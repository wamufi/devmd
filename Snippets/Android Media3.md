---
title: Android Media3
date:  2025-12-28
time:  18:11:43
tags:
  - android
  - kotlin
  - compose
---

# 🎼 Android Media3 & Compose 전체 연동 가이드 (종합)

## 1. 알림창 클릭 및 화면 진입 로직 (MainActivity)

알림창을 눌러 앱으로 돌아올 때 음악이 끊기지 않고 플레이어 화면만 부드럽게 올리는 방법입니다.

- **문제:** 알림 클릭 시 `MainActivity`가 새로 뜨거나 `onNewIntent`가 호출될 때 재생 로직이 다시 실행되는 문제.
    
- **해결:** `Intent`를 분석하여 플레이어 화면을 보여주는 **상태값(`showPlayerView`)**만 변경하고, 실제 재생 명령은 호출하지 않습니다.
    

Kotlin

```
// MainActivity.kt
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    handleIntent(intent) // 앱이 죽어있을 때 클릭 시
}

override fun onNewIntent(intent: Intent) {
    super.onNewIntent(intent)
    handleIntent(intent) // 앱이 살아있을 때 클릭 시
}

private fun handleIntent(intent: Intent?) {
    if (intent?.action == "ACTION_SHOW_PLAYER") {
        // ViewModel의 UI 노출 상태만 변경 (음악 재생 play() 호출 X)
        viewModel.showPlayer() 
    }
}
```

---

## 2. 플레이어 화면 최적화 (PlayerScreen)

`AnimatedVisibility`를 사용하여 화면을 띄울 때의 주의사항입니다.

- **문제:** `PlayerScreen`이 보일 때마다 내부의 `LaunchedEffect`가 재실행되어 `play()`가 중복 호출됨 (음악 끊김 및 파일 에러 발생).
    
- **해결:** **UI 레이어에서 재생 로직을 완전히 제거**합니다.
    

Kotlin

```
// UI 레이어는 '그리기'만 담당합니다.
AnimatedVisibility(visible = viewModel.showPlayerView) {
    PlayerScreen() // 내부에서 LaunchedEffect(Unit) { viewModel.play() } 금지!
}
```

---

## 3. ViewModel 기반 데이터 및 상태 관리

모든 재생 로직과 상태 업데이트는 `ViewModel`에서 관리합니다.

### A. 곡 선택 및 재생 (Entrance)

Kotlin

```
fun updateStreamingUrl(url: String, title: String) {
    if (url.isBlank() || _uiState.value.streamingUrl == url) return
    
    _uiState.update { it.copy(streamingUrl = url, title = title) }
    // 새로운 곡일 때만 딱 한 번 실행
    play(url, title) 
}
```

### B. 컨트롤러 연결 및 초기 동기화 (Setup)

알림창에서 복귀했을 때 멈춰있는 UI를 깨우는 핵심 로직입니다.

Kotlin

```
private fun setupController() {
    val controller = mediaController ?: return
    controller.addListener(this) // onEvents 리스너 등록

    // [핵심] 복귀 시점의 현재 정보를 UI에 즉시 주입
    refreshUI(controller)
    
    // [핵심] 이미 재생 중이라면 멈춰있는 티커를 다시 시작
    if (controller.isPlaying) {
        startTicker()
    }
}
```

### C. 상태 변화 감지 및 티커 (onEvents)

Kotlin

```
override fun onEvents(player: Player, events: Player.Events) {
    _uiState.update { currentState ->
        var newState = currentState
        
        // 1. 메타데이터(제목 등) 동기화
        if (events.contains(Player.EVENT_MEDIA_METADATA_CHANGED)) {
            newState = newState.copy(title = player.mediaMetadata.title?.toString() ?: "")
        }
        
        // 2. 재생/일시정지 상태에 따른 티커 제어
        if (events.contains(Player.EVENT_IS_PLAYING_CHANGED)) {
            newState = newState.copy(isPlaying = player.isPlaying)
            if (player.isPlaying) startTicker() else stopTicker()
        }
        
        // 3. 버퍼링 및 총 길이 업데이트
        newState.copy(
            durationMs = player.duration.takeIf { it > 0 } ?: currentState.durationMs,
            bufferedMs = player.bufferedPosition
        )
    }
}

private fun startTicker() {
    positionUpdateJob?.cancel() // 기존 루프 종료 후 새 루프 시작 (최신 컨트롤러 참조)
    positionUpdateJob = viewModelScope.launch {
        while (isActive) {
            mediaController?.let { controller ->
                if (controller.isPlaying) {
                    _uiState.update { it.copy(positionMs = controller.currentPosition) }
                }
            }
            delay(500L)
        }
    }
}
```

---

## 4. 부가 기능 (꿀팁)

- **출처 판별:** `player.currentMediaItem?.localConfiguration?.uri.toString()`을 통해 현재 곡이 웹 URL인지 로컬 파일인지 확인 가능합니다.
    
- **종료 처리:** 자동 재생이 없는 경우 `STATE_ENDED` 이벤트 시 티커를 멈추고 `positionMs`를 `duration`으로 고정하여 SeekBar를 끝까지 채워줍니다.
    

---

### 💡 저장하실 때 참고하세요!

이 구조의 핵심은 **"UI는 서비스의 상태를 관찰할 뿐, 스스로 명령하지 않는다"**는 것입니다. 이렇게 분리하면 알림창, 헤드셋 버튼, 외부 앱 연결 등 어떤 상황에서도 음악이 끊기지 않고 UI가 정확하게 동기화됩니다.

정리가 잘 되었나요? 추가로 특정 파일 형태나 복잡한 재생 목록 기능이 필요해지면 언제든 말씀해 주세요! ☺️



# 📱 Android Media3 플레이어 연동 및 UI 동기화 가이드

## 1. 핵심 문제 및 해결 전략

- **문제:** 알림창 클릭 등으로 앱에 복귀할 때 `PlayerScreen`이 다시 그려지며 `LaunchedEffect`가 재실행되어 음악이 끊기거나, `MediaController` 연결 시 이벤트가 발생하지 않아 SeekBar가 멈추는 현상.
    
- 해결: 1. UI 레이어 재생 로직 제거: PlayerScreen 내의 LaunchedEffect에서 play()를 호출하지 않음.
    
    2. ViewModel 중심 제어: 재생 명령은 곡을 선택하는 시점에만 내리고, UI는 StateFlow를 통해 상태만 보여줌.
    
    3. 초기 동기화 강제: MediaController가 연결되는 즉시(setupController) 현재 메타데이터와 재생 상태를 수동으로 갱신.
    
    4. 티커(Ticker) 재시작: 기존 티커를 취소(cancel)하고 최신 컨트롤러를 참조하는 새 루틴을 실행.
    

---

## 2. ViewModel 핵심 코드 (Kotlin)

Kotlin

```
@HiltViewModel
class PlayerViewModel @Inject constructor(
    private val context: Context
) : ViewModel() {

    private val _uiState = MutableStateFlow(PlayerUiState())
    val uiState: StateFlow<PlayerUiState> = _uiState.asStateFlow()

    private var controllerFuture: ListenableFuture<MediaController>? = null
    private val mediaController: MediaController? get() = if (controllerFuture?.isDone == true) controllerFuture?.get() else null
    private var positionUpdateJob: Job? = null

    /**
     * 1. 곡 선택 시 호출 (중복 재생 방지 및 입구 일원화)
     */
    fun updateStreamingUrl(newUrl: String, title: String) {
        if (newUrl.isBlank() || _uiState.value.streamingUrl == newUrl) return
        
        _uiState.update { it.copy(streamingUrl = newUrl, title = title) }
        play(newUrl, title)
    }

    /**
     * 2. 컨트롤러 초기화 및 리스너 설정
     */
    private fun setupController() {
        val controller = mediaController ?: return
        controller.addListener(object : Player.Listener {
            override fun onEvents(player: Player, events: Player.Events) {
                // UI 상태 일괄 업데이트 로직
                updateUIFromPlayer(player, events)
            }
        })

        // [중요] 알림창 복귀 시 초기 동기화 강제 실행
        refreshUI(controller)
        
        // [중요] 이미 재생 중이라면 티커 즉시 실행
        if (controller.isPlaying) {
            startTicker()
        }
    }

    /**
     * 3. 재생 위치 업데이트 (티커)
     */
    private fun startTicker() {
        // 기존 작업이 있다면 취소하고 최신 컨트롤러 기준으로 새로 시작
        positionUpdateJob?.cancel()
        positionUpdateJob = viewModelScope.launch {
            while (isActive) {
                mediaController?.let { controller ->
                    if (controller.isPlaying) {
                        _uiState.update { it.copy(positionMs = controller.currentPosition) }
                    }
                }
                delay(500L) // 0.5초 간격 갱신
            }
        }
    }

    private fun updateUIFromPlayer(player: Player, events: Player.Events) {
        _uiState.update { currentState ->
            var newState = currentState
            
            // 메타데이터(제목 등) 변경 시
            if (events.contains(Player.EVENT_MEDIA_METADATA_CHANGED)) {
                newState = newState.copy(
                    title = player.mediaMetadata.title?.toString() ?: currentState.title
                )
            }
            
            // 재생 상태 변경 시 티커 제어
            if (events.contains(Player.EVENT_IS_PLAYING_CHANGED)) {
                newState = newState.copy(isPlaying = player.isPlaying)
                if (player.isPlaying) startTicker() else positionUpdateJob?.cancel()
            }

            // 곡 길이 및 버퍼링 업데이트
            newState.copy(
                durationMs = player.duration.takeIf { it > 0 } ?: currentState.durationMs,
                bufferedMs = player.bufferedPosition
            )
        }
    }
}
```

---

## 3. UI 레이어 (Compose)

Kotlin

```
@Composable
fun PlayerScreen(viewModel: PlayerViewModel) {
    val uiState by viewModel.uiState.collectAsState()

    Column {
        // 1. 제목 표시 (초기화 및 복귀 시 refreshUI 덕분에 즉시 표시됨)
        Text(text = uiState.title)

        // 2. SeekBar (티커 덕분에 알림창 복귀 후에도 실시간 흐름 유지)
        Slider(
            value = uiState.positionMs.toFloat(),
            valueRange = 0f..uiState.durationMs.toFloat(),
            onValueChange = { /* Seek 로직 */ }
        )

        // 3. 재생/일시정지 버튼
        IconButton(onClick = { viewModel.togglePlay() }) {
            Icon(if (uiState.isPlaying) Icons.Pause else Icons.Play)
        }
    }
}
```

---

## 4. 요약 설명 (저장용)

1. **재생 주소 판별:** `currentMediaItem.localConfiguration.uri.toString()`을 통해 현재 소스가 URL인지 로컬 파일인지 확인할 수 있습니다.
    
2. **onEvents 활용:** 여러 상태(재생 여부, 메타데이터, 버퍼링)를 개별 콜백보다 `onEvents`에서 묶어서 처리하는 것이 UI 정합성에 유리합니다.
    
3. **티커 안정성:** `isActive` 체크 대신 `cancel()` 후 새 `launch`를 사용하는 패턴은 컨트롤러 인스턴스 교체 상황(복귀 시)에서 가장 안전합니다.
    
4. **수동 선택 구조:** 자동 다음 곡 재생이 없는 경우, 곡이 끝났을 때(`STATE_ENDED`) 티커를 명시적으로 종료해주는 로직이 포함되었습니다.
    

---

이 내용을 정리해서 저장해 두시면 나중에 비슷한 미디어 기능을 구현하실 때 큰 도움이 될 거예요! 추가로 궁금한 점이 생기면 언제든 물어봐 주세요.