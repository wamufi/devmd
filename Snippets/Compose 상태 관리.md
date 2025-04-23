---
title: Compose 상태 관리
date:  2025-04-23
time:  04:11:52
tags:
  - android
  - kotlin
  - compose
---
### 🟠 `rememberUpdatedState`

> 🔹 **람다나 값이 변경되어도 최신 상태를 기억해서 사용할 때**  
> 🔹 특히 **`LaunchedEffect`, `SideEffect`, 코루틴** 블록 안에서 람다를 참조할 때 유용

#### 예시

> ✅ **BottomSheet에서 선택한 값을 전달해서 타이머 설정을 할 때**

```kotlin
@Composable
fun TimerScreen(onTimeout: () -> Unit) {
    val updatedCallback by rememberUpdatedState(onTimeout)

    LaunchedEffect(Unit) {
        delay(3000)
        updatedCallback() // 최신 콜백 실행
    }
}
```

📌 만약 `onTimeout`이 변경되었는데 `LaunchedEffect`가 다시 실행되지 않아야 한다면 → `rememberUpdatedState`없으면 이전 값으로 실행됨!

#### 왜 필요해?

- `onTimeout`이 외부에서 바뀌어도, `LaunchedEffect`가 재실행되지 않게 하면서도 **최신 콜백**을 써야 할 때.
    

---

### 🟡 `LaunchedEffect`

> 🔹 **특정 key가 변경될 때만 실행되는 코루틴 블록**  
> 🔹 side-effect (데이터 로딩, 애니메이션 시작 등)에 사용

#### 예시

> ✅ **DataStore에서 폰트 사이즈 로드할 때**

```kotlin
@Composable
fun FontSettingScreen(viewModel: FontViewModel) {
    val fontSize by viewModel.fontSize.collectAsState()

    LaunchedEffect(fontSize) {
        Log.d("Font", "Font size changed: $fontSize")
    }

    Text("Preview", fontSize = fontSize.sp)
}
```

📌 `fontSize`가 바뀔 때마다 부가 처리(로깅, 서버 통신 등) 하고 싶을 때.

#### 주요 포인트

- `LaunchedEffect(Unit)` → 처음 진입할 때 한 번만 실행
- `LaunchedEffect(key1)` → key가 바뀌면 재실행됨

---

### 🟢 `derivedStateOf`

> 🔹 **기존 상태를 기반으로 계산된 값을 효율적으로 관리**  
> 🔹 성능 최적화용 (계산 결과 캐시)

#### 예시

> ✅ **화면에서 필터링된 리스트 개수만 보여주고 싶을 때**

```kotlin
@Composable
fun FilteredList(viewModel: ItemViewModel) {
    val items by viewModel.items.collectAsState()

    val filteredCount by remember {
        derivedStateOf { items.count { it.selected } }
    }

    Text("Selected: $filteredCount")
}
```

📌 items가 바뀌면 자동으로 필터링해서 count, recomposition 최소화.

#### 왜 써?

- `items`가 변경될 때만 `itemCount`를 **자동으로 계산**하고 recomposition을 유발.
    
- 다른 상태를 기반으로 파생 상태를 만들 때 유용.
    

---

### 요약 표

|이름|언제 사용해?|특징|
|---|---|---|
|`rememberUpdatedState`|코루틴 블록에서 최신 람다/값이 필요할 때|최신 상태 기억|
|`LaunchedEffect`|key 변화에 따라 코루틴 실행|side-effect 실행|
|`derivedStateOf`|다른 상태에서 파생된 값을 만들 때|성능 최적화용 캐싱|