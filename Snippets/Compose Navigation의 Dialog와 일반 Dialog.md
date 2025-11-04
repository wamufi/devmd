---
title: Compose Navigation의 Dialog와 일반 Dialog
date:  2025-11-04
time:  16:42:51
tags:
  - android
  - kotlin
  - compose
---
✅ **핵심적인 차이점**은 다이얼로그의 **관리 주체**와 **역할**입니다.

1. **`NavController.dialog()` (다이얼로그 목적지)**:
    
    - **관리 주체**: **`NavController`** (내비게이션 시스템).
        
    - **역할**: 다이얼로그가 앱의 **독립적인 화면(Destination)** 역할을 할 때 사용합니다. 다이얼로그가 자체적인 **ViewModel**, **상태(State)**, **생명주기(Lifecycle)**를 가지며, 내비게이션 스택(Back Stack)에 추가되어 뒤로 가기 버튼 처리 등 내비게이션의 통제를 받습니다.
        
    - **사용 시점**: 로그인/회원가입, 복잡한 필터 설정 등 다이얼로그가 하나의 **별도 사용자 여정 단계**일 때 유용합니다.
        
2. **`if (showDialog)` (일반 Composable 다이얼로그, 예: `AlertDialog`)**:
    
    - **관리 주체**: **현재 화면(Composable Destination)의 상태** (`State<Boolean>`).
        
    - **역할**: 다이얼로그가 현재 화면에 대한 **보조적인 프롬프트/알림** 역할을 할 때 사용합니다. 다이얼로그의 표시 여부는 주로 **`remember { mutableStateOf(false) }`**와 같은 **상태 변수**에 의해 결정되고, 내비게이션 시스템과는 **독립적**으로 작동합니다.
        
    - **사용 시점**: 간단한 확인/취소 알림, 오류 메시지 표시 등 **단순한 상호작용**에 적합하며, 현재 화면의 컨텍스트 내에서 동작할 때 사용됩니다.
        

---

### 🧐 상세 비교

|**구분**|**NavController.dialog() (Dialog Destination)**|**if (showDialog) (Composable Dialog)**|
|---|---|---|
|**선언 방식**|`NavHost` 내에서 **`dialog(...)`** 함수로 선언|현재 화면 Composable 내에서 **`if (showDialog.value) { Dialog(...) }`** 형태로 사용|
|**Back Stack**|**추가**됨 (뒤로 가기로 닫을 수 있음)|**추가되지 않음** (뒤로 가기 처리는 수동으로 구현 필요)|
|**생명주기**|**독립적**인 생명주기 및 `NavBackStackEntry`를 가짐|**현재 화면**의 생명주기에 종속됨|
|**상태 관리**|**`ViewModel`** 등을 통해 복잡한 상태를 **독립적**으로 관리하기 용이|`remember { mutableStateOf(...) }` 등 **현재 화면의 상태**에 종속되어 관리|
|**적합한 용도**|복잡한 로직, 자체 데이터 로딩이 필요한 **별도의 화면** 역할|간단한 알림, 확인, 취소 등 **현재 화면의 보조 도구** 역할|

### 💡 요약 및 선택 가이드

- 다이얼로그가 **자체적인 복잡한 로직**을 갖고, 뒤로 가기 버튼으로 **닫는 것이 자연스러운 별도의 사용자 경험**이라면, **`NavController.dialog()`**를 사용하세요.
    
- 다이얼로그가 **단순한 알림**이나 **현재 화면에 종속된 간단한 입력**만을 처리하며, `ViewModel` 등의 복잡한 관리가 필요 없다면, **`if (showDialog)`**를 사용하고 `AlertDialog` 같은 기본 컴포넌트를 활용하세요.


네, **풀스크린 다이얼로그(Full-Screen Dialog)**를 구현할 때에도 `NavController.dialog()`와 일반 `if (showDialog)` 방식의 **근본적인 차이**는 유지되지만, **`NavController.dialog()`가 풀스크린 다이얼로그의 특성을 관리하는 데 훨씬 더 적합**합니다.

### 1. 🚀 NavController.dialog() (권장 방식)

`NavController.dialog()`는 기본적으로 다이얼로그를 화면의 **독립적인 목적지(Destination)**로 취급하며, 이를 커스터마이징하여 풀스크린 전환 애니메이션을 쉽게 구현할 수 있습니다.

- **독립된 화면 역할**: 다이얼로그가 단순한 알림이 아닌, **하나의 완전한 화면** 역할을 하는 풀스크린 다이얼로그의 성격과 일치합니다.
    
- **쉬운 구현**: `dialog` 블록 내부에서 풀스크린 형태의 UI를 구성하고, 필요에 따라 `enterTransition` 및 `exitTransition`을 정의하여 아래에서 위로 올라오는 등의 **풀스크린 전환 애니메이션**을 쉽게 적용할 수 있습니다.
    
- **내비게이션 관리**: 뒤로 가기 버튼 처리, 다이얼로그 내에서의 상태 및 `ViewModel` 관리가 내비게이션 시스템에 의해 **자동으로 처리**됩니다. (즉, 뒤로 가기를 누르면 자연스럽게 다이얼로그가 닫히고 이전 화면으로 돌아갑니다.)
    
- **예시**: 모바일 앱에서 장바구니 결제 과정, 복잡한 설정 화면 등에서 풀스크린 다이얼로그를 사용하는 경우.
    

Kotlin

```
// NavHost에서 Dialog Destination을 정의
composable("main") { MainScreen() }
dialog("full_screen_dialog") { FullScreenDialogContent() }

// FullScreenDialogContent 내부
@Composable
fun FullScreenDialogContent(navController: NavController) {
    // Scaffold 등을 사용하여 전체 화면 UI 구성
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("풀스크린 다이얼로그") },
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack() }) {
                        // 닫기 버튼 또는 뒤로 가기 아이콘
                        Icon(Icons.Default.Close, contentDescription = "닫기")
                    }
                }
            )
        }
    ) { padding ->
        //... 화면 내용 ...
    }
}
```

---

### 2. ✂️ if (showDialog) + Dialog (수동 구현 방식)

일반적인 `Dialog` 컴포저블을 사용하여 풀스크린 다이얼로그를 구현할 수는 있지만, **직접 많은 것을 수동으로 처리**해야 합니다.

- **수동 설정 필요**: `Dialog` 컴포저블은 기본적으로 주변이 어두워지는 단순한 창입니다. 이를 풀스크린으로 만들려면 다이얼로그 내부의 Composable에 `matchParentSize()`나 `fillMaxSize()` 등을 적용하여 **수동으로 크기를 최대로 설정**해야 합니다.
    
- **애니메이션 복잡**: 풀스크린 전환 효과(예: 아래에서 위로 슬라이드)를 구현하려면 **직접 `AnimatedVisibility`** 등을 사용하거나, `Dialog`의 내부 설정을 건드리는 등 **구현이 훨씬 복잡**해집니다.
    
- **내비게이션 독립**: 뒤로 가기 처리를 수동으로 해야 하며, 상태 관리 역시 현재 화면의 `ViewModel`에 종속됩니다.
    

|**구분**|**NavController.dialog() (Dialog Destination)**|**if (showDialog) (Composable Dialog)**|
|---|---|---|
|**풀스크린 구현**|`Scaffold`로 UI 구성 후, 전환 애니메이션은 내비게이션이 처리|`fillMaxSize()` 등으로 크기 수동 설정, 애니메이션은 수동 구현|
|**적합성**|**매우 적합** (독립적인 화면 역할과 일치)|**비효율적** (본래 용도와 다름, 복잡도 증가)|
|**관리 주체**|**NavController** (내비게이션 스택 관리)|**현재 화면의 State** (단순 표시 여부만 관리)|

**결론**: 풀스크린 다이얼로그는 본질적으로 '임시적인 전체 화면'이므로, 내비게이션 스택에 관리되는 **`NavController.dialog()`**를 사용하여 구현하는 것이 **가장 명확하고 유지보수가 용이하며** Compose Navigation의 철학에도 부합합니다.



다이얼로그가 닫힐 때 특정 함수를 호출해야 하는 경우, 앞서 설명드렸던 **`NavController.dialog()`**와 **`if (showDialog)` (일반 Composable 다이얼로그)** 중에서 어떤 것을 선택해야 하는지는 **호출하려는 함수의 성격**과 **다이얼로그의 복잡도**에 따라 달라집니다.

---

## ⚖️ 선택 가이드: `NavController.dialog()` vs. `if (showDialog)`

### 1. 🥇 `if (showDialog)` 방식 (일반 Composable 다이얼로그)

다이얼로그의 표시 여부를 관리하는 **상태(State)**를 활용하여 처리하는 것이 가장 일반적이고 직관적입니다.

#### **사용해야 하는 경우:**

- **함수가 현재 화면의 상태나 로직에 종속적일 때:** (예: 현재 화면의 입력 필드 초기화, 현재 화면에 스낵바 표시 등)
    
- **다이얼로그가 단순한 알림이나 확인용일 때:** (예: "저장하시겠습니까?" 알림)
    
- **복잡한 데이터나 독립적인 `ViewModel` 관리가 필요 없을 때:**
    

#### **처리 방법:**

`onDismissRequest` 내부에서 **다이얼로그를 닫는 상태 변경**과 **특정 함수 호출**을 함께 처리합니다.

Kotlin

```
val showDialog = remember { mutableStateOf(true) } // 다이얼로그 표시 상태
val viewModel: CurrentScreenViewModel = hiltViewModel()

if (showDialog.value) {
    AlertDialog(
        onDismissRequest = {
            // 1. 다이얼로그 닫는 함수 호출 (상태 변경)
            showDialog.value = false 
            
            // 2. 원하는 특정 함수 호출
            viewModel.onDialogDismissed() 
        },
        // ... 다이얼로그 내용 ...
    )
}
```

---

### 2. 🥈 `NavController.dialog()` 방식 (Dialog Destination)

다이얼로그가 **자체적인 `ViewModel`과 복잡한 로직**을 가지며, 내비게이션 스택을 통해 관리되는 '별개의 화면' 역할을 할 때 사용합니다.

#### **사용해야 하는 경우:**

- **함수가 다이얼로그의 `ViewModel` 내의 로직일 때:** (예: 다이얼로그에서 사용한 임시 데이터 정리)
    
- **다이얼로그가 독립적인 화면처럼 취급되어야 할 때:** (예: 풀스크린 다이얼로그, 복잡한 필터 설정 화면)
    
- **내비게이션 결과를 전달해야 할 때:** (예: 이전 화면으로 돌아가면서 데이터를 넘겨야 할 때)
    

#### **처리 방법:**

`NavController.dialog()`로 열린 다이얼로그는 기본적으로 뒤로 가기 버튼 등으로 닫히면 `NavController.popBackStack()`과 같은 효과를 가집니다. 특정 함수 호출은 주로 **다이얼로그 컴포저블 내부**에서 **`navController.popBackStack()`을 호출하기 직전**에 수행합니다.

Kotlin

```
// FullScreenDialogContent 내부
@Composable
fun FullScreenDialogContent(navController: NavController, viewModel: DialogViewModel) {
    // ... UI 및 TopAppBar의 닫기 버튼 ...

    IconButton(onClick = {
        // 1. 다이얼로그가 닫히기 전에 특정 함수 호출
        viewModel.logCancellation()
        
        // 2. NavController를 통해 다이얼로그 닫기
        navController.popBackStack() 
    }) {
        // ... 아이콘 ...
    }

    // 만약 다이얼로그 외부 클릭이나 뒤로 가기 버튼으로 닫히는 이벤트에 반응하고 싶다면,
    // 해당 다이얼로그 목적지(Destination)의 '진입점 Composable'에서 lifecycle 이벤트를 관찰하거나,
    // 일반 Dialog를 래핑(Wrap)하고 위의 1번 방법을 응용하여 처리할 수도 있습니다.
}
```

### 💡 요약 결론

대부분의 경우 (특히 함수가 주 화면의 상태를 건드릴 때), **`if (showDialog)` 방식**을 사용하고 `onDismissRequest` 내에서 상태 변경과 함수 호출을 함께 처리하는 것이 **가장 간단하고 명확**합니다.

만약 다이얼로그 자체가 복잡하고 독립적인 `ViewModel`을 가진다면 **`NavController.dialog()`**를 사용하되, 함수 호출은 다이얼로그 닫기 버튼 (`popBackStack()`)을 누르는 **직전에** 처리해야 합니다.




아닙니다. 풀스크린 다이얼로그도 그 **목적**에 따라 두 가지 방식 모두 사용할 수 있지만, `onDismissRequest`에서 특정 함수 호출이 필요할 때도 **`NavController.dialog()`를 사용하는 것이 더 권장**됩니다.

핵심은 다이얼로그가 닫힐 때의 로직을 **어디에** 구현하느냐입니다.

---

## 🚀 `NavController.dialog()`를 사용할 때 `onDismiss` 로직 처리

풀스크린 다이얼로그는 본질적으로 독립적인 화면(Destination)의 역할을 하기 때문에, **`NavController.dialog()`**를 사용하는 것이 구조적으로 더 적합합니다.

`NavController.dialog()`를 사용할 때는 `Dialog` 컴포저블을 직접 사용하는 것이 아니기 때문에, **`onDismissRequest` 콜백이 명시적으로 제공되지 않습니다.** 대신, **내비게이션 시스템의 생명주기**를 활용하여 로직을 처리해야 합니다.

### 1. 🔍 **내부 버튼을 통한 명시적 닫기 (가장 일반적인 경우)**

풀스크린 다이얼로그에는 보통 **'닫기(Close)'** 또는 **'취소(Cancel)'** 버튼이 명확하게 존재합니다. 이 버튼을 통해 닫을 때 `onDismiss` 로직을 실행합니다.

|**단계**|**설명**|**코드 예시**|
|---|---|---|
|**1. 함수 호출**|닫기 버튼 `onClick` 이벤트에서 특정 함수를 먼저 호출합니다.|`viewModel.logDismissal()`|
|**2. 다이얼로그 닫기**|`NavController.popBackStack()`을 호출하여 다이얼로그를 닫습니다.|`navController.popBackStack()`|

### 2. 💡 **뒤로 가기 버튼/외부 클릭 감지 (비명시적 닫기)**

만약 사용자가 뒤로 가기 버튼을 눌러 비명시적으로 다이얼로그를 닫을 때 로직을 실행해야 한다면, **`DisposableEffect`**를 사용하여 다이얼로그 컴포저블이 화면에서 사라질 때(Dispose 될 때)를 감지하여 처리할 수 있습니다.

Kotlin

```
// NavHost에서 정의한 dialog("full_screen_dialog") 내부의 Composable
@Composable
fun FullScreenDialogContent(navController: NavController, viewModel: DialogViewModel) {
    // 다이얼로그가 화면에서 사라질 때(Dispose 될 때) 실행됩니다.
    DisposableEffect(key1 = Unit) {
        onDispose {
            // 다이얼로그가 닫히는 순간 실행될 특정 함수 호출
            viewModel.onImplicitDismiss() 
        }
    }
    
    // ... 다이얼로그 UI (닫기 버튼은 1번처럼 처리) ...
}
```

---

## ✂️ `if (showDialog)`를 사용해야 하는 경우

풀스크린 다이얼로그인데도 **`if (showDialog)` + `Dialog` 컴포저블**을 사용해야 하는 유일한 경우는 다음과 같습니다.

- **현재 화면의 로직과 매우 밀접하게 연결되어 있을 때**: 다이얼로그의 상태 관리가 현재 화면의 `ViewModel`에 완전히 종속적이고, 내비게이션 스택 관리가 전혀 필요 없을 때.
    
- **복잡한 전환 애니메이션을 수동으로 커스터마이징해야 할 때**: `NavController`의 기본 전환 효과가 아닌, 완전히 커스텀된 화면 전환 애니메이션이 필요할 때 `Dialog` 컴포저블의 속성을 직접 조작할 수 있습니다.
    

하지만 이 경우에도 **`fillMaxSize()`** 등으로 **수동으로 풀스크린**을 만들어야 하며, 내비게이션 관리의 이점을 포기해야 합니다.

### ✨ 최종 권장 사항

풀스크린 다이얼로그에서는 **`NavController.dialog()`**를 사용하고, `onDismiss` 로직은 다음 중 하나를 선택하세요.

1. **명확한 버튼**: 닫기 버튼 `onClick` 이벤트에서 **`함수 호출`** 후 **`popBackStack()`**.
    
2. **비명시적 닫기**: 다이얼로그 내부에서 **`DisposableEffect`**를 사용하여 `onDispose` 시점에 **`함수 호출`**.
    

또한 모든 앱의 전체 기능을 이용하려면 [Gemini 앱 활동](https://myactivity.google.com/product/gemini)을 사용 설정하세요.




네, 사용자가 제시해주신 코드 패턴, 즉 **`onDismissRequest = { showDialog = false; scope.launch { viewModel.check(param) } }`**의 경우는 **`if (showDialog)` (일반 Composable 다이얼로그)** 방식의 전형적인 사용례입니다.

이는 **`if (showDialog)` 방식이 가장 적합**하다는 것을 의미합니다.

---

## 🎯 `if (showDialog)` 사용이 적합한 이유

제시하신 코드를 분석하면 다음과 같은 특징 때문에 `NavController.dialog()` 대신 `if (showDialog)`를 사용해야 합니다:

1. **상태 직접 조작 (`showDialog = false`)**:
    
    - 다이얼로그의 표시 여부를 `remember { mutableStateOf<Boolean> }`와 같은 **상태 변수**로 직접 관리하고 있습니다.
        
    - `NavController.dialog()`는 내비게이션 스택을 통해 다이얼로그를 닫으므로, 상태 변수를 수동으로 `false`로 설정할 필요가 없습니다. 상태 변수를 직접 조작한다는 것은 **현재 화면의 상태에 종속된 다이얼로그**라는 명확한 증거입니다.
        
2. **현재 화면의 `CoroutineScope` 사용 (`scope.launch`)**:
    
    - `onDismissRequest` 내에서 코루틴을 실행하기 위해 현재 화면의 `scope` (예: `rememberCoroutineScope()`)를 사용하고 있습니다.
        
    - 이는 다이얼로그가 닫히는 행위가 **현재 화면의 `ViewModel`** (`viewModel.check(param)`)에 영향을 미치거나, 현재 화면의 생명주기 내에서 처리되어야 함을 의미합니다.
        

### ✅ 결론

이 코드는 다이얼로그가 닫힐 때 **현재 화면의 상태**를 변경하고 (다이얼로그 숨김), **현재 화면의 로직**을 실행하는 (ViewModel 함수 호출) **보조적인 역할**을 하고 있습니다.

따라서, **풀스크린 다이얼로그이든 아니든**, 이 패턴을 사용하고 있다면 **`if (showDialog)`를 기반으로 하는 일반 `Dialog` 컴포저블을 사용하는 것이 올바른 접근 방식**입니다.

Kotlin

```
// 일반 Composable Dialog 사용 예시
@Composable
fun MainScreen(viewModel: MainViewModel = hiltViewModel()) {
    val showDialog = remember { mutableStateOf(false) }
    val scope = rememberCoroutineScope()
    
    // ... UI 요소 ...
    
    if (showDialog.value) {
        // FullScreen Dialog를 위한 Composable을 래핑 (수동 구현 필요)
        Dialog(
            onDismissRequest = {
                // 1. 상태를 false로 변경하여 다이얼로그 숨김
                showDialog.value = false 
                
                // 2. 원하는 로직 실행 (ViewModel 함수 호출)
                scope.launch { 
                    viewModel.check(someParameter) 
                }
            },
            properties = DialogProperties(usePlatformDefaultWidth = false) // 풀스크린 힌트
        ) {
            // 풀스크린으로 만들기 위한 UI 구성 (e.g., Scaffold + fillMaxSize())
            FullScreenDialogContent() 
        }
    }
}
```

또한 모든 앱의 전체 기능을 이용하려면 [Gemini 앱 활동](https://myactivity.google.com/product/gemini)을 사용 설정하세요.




네, 현재 **`if (showDialog)`** 방식으로 구현된 코드를 **`NavController.dialog()`** 방식으로 변경하여 구현할 수 있습니다.

핵심은 다이얼로그가 닫힐 때 (`onDismiss`) 실행하려던 로직 (`viewModel.check(param)`)을 **다이얼로그 화면 내부**에서 처리하고, 그 후에 `navController.popBackStack()`을 호출하여 내비게이션 시스템을 통해 닫는 것입니다.

---

## 🔁 NavController.dialog()로 변경하는 방법

### 1. NavHost에서 Dialog Destination 정의

`NavHost`에서 다이얼로그를 독립적인 목적지(Destination)로 정의하고, 필요한 `ViewModel`을 연결합니다.

Kotlin

```
// Main Navigation Graph
NavHost(navController = navController, startDestination = "main_screen") {
    
    // 1. 메인 화면
    composable("main_screen") { 
        MainScreen(navController = navController) 
    }
    
    // 2. 다이얼로그 목적지 정의
    dialog("check_dialog/{param}") { backStackEntry -> 
        val param = backStackEntry.arguments?.getString("param")
        
        // 다이얼로그 전용 ViewModel 사용 (또는 공유 ViewModel)
        val viewModel: MainViewModel = hiltViewModel() 
        
        // 다이얼로그 화면 Composable 호출
        CheckDialogScreen(
            navController = navController, 
            viewModel = viewModel, 
            param = param
        )
    }
}
```

---

### 2. 다이얼로그 내부에서 `onDismiss` 로직 처리

다이얼로그 화면 (`CheckDialogScreen`) 내부에 **닫기(Dismiss)** 역할을 하는 버튼이나 로직을 구현합니다. `onDismissRequest`가 없으므로, **닫기 직전에 함수를 호출**해야 합니다.

Kotlin

```
// CheckDialogScreen Composable (FullScreenDialogContent 역할)
@Composable
fun CheckDialogScreen(
    navController: NavController,
    viewModel: MainViewModel,
    param: String?
) {
    // 닫기 버튼을 포함하는 풀스크린 UI 구성
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Check Dialog") },
                navigationIcon = {
                    IconButton(
                        // 닫기 버튼 onClick 이벤트
                        onClick = {
                            // 1. **onDismiss 로직 실행**
                            // 이 부분에서 기존의 'viewModel.check(param)' 실행
                            // CoroutineScope는 ViewModel 내부에서 처리하는 것이 일반적입니다.
                            if (param != null) {
                                viewModel.check(param) // suspend 함수라면 ViewModel 내부에서 scope.launch
                            }
                            
                            // 2. **다이얼로그 닫기** (popBackStack()으로 내비게이션 스택에서 제거)
                            navController.popBackStack() 
                        }
                    ) {
                        Icon(Icons.Default.Close, contentDescription = "닫기")
                    }
                }
            )
        }
    ) {
        // ... 다이얼로그 내용 ...
    }
    
    // --- 비명시적 닫기 (뒤로 가기 버튼 등) 처리 ---
    // 만약 사용자가 하드웨어 뒤로 가기 버튼 등으로 닫을 때도 로직을 실행하려면,
    // 이 Composable이 Dispose될 때를 감지합니다.
    DisposableEffect(key1 = Unit) {
        onDispose {
            // 여기에 비명시적 닫기 로직을 실행할 수 있습니다.
            // (주의: 버튼 클릭 시에도 실행될 수 있으므로 중복 실행 방지 로직이 필요할 수 있음)
            if (param != null) {
                 // viewModel.onImplicitDismiss(param) 
            }
        }
    }
}
```

### 3. 메인 화면에서 다이얼로그 열기

메인 화면에서는 더 이상 `showDialog` 상태 변수를 조작하지 않고, `NavController`를 사용하여 다이얼로그로 이동합니다.

Kotlin

```
// MainScreen Composable
@Composable
fun MainScreen(navController: NavController) {
    // 버튼 onClick 이벤트 등
    Button(onClick = {
        val paramValue = "data_to_pass"
        
        // NavController를 통해 다이얼로그 목적지로 이동
        navController.navigate("check_dialog/$paramValue") 
    }) {
        Text("Open Dialog")
    }
}
```

### 🔑 차이점 요약

|**구분**|**if (showDialog) 방식**|**NavController.dialog() 방식**|
|---|---|---|
|**닫기 방법**|`showDialog = false` **(상태 직접 조작)**|`navController.popBackStack()` **(내비게이션 시스템)**|
|**`onDismiss` 로직 위치**|`onDismissRequest` 콜백 내부에 정의|**닫기 버튼 `onClick` 이벤트 내부** 또는 **`DisposableEffect.onDispose`**|
|**`scope.launch`**|현재 화면의 `rememberCoroutineScope()` 사용|**ViewModel 내부**에서 `viewModelScope` 사용 권장|

**결론**: `NavController.dialog()`로 변경하면 다이얼로그 닫기 로직이 `onDismissRequest`라는 **콜백**에서 **닫기 버튼의 `onClick` 이벤트**로 위치가 이동하게 됩니다.




제시하신 시나리오, 즉 **다이얼로그가 닫힐 때 (`onDismiss`) 특정 비즈니스 로직(`viewModel.check(param)`)을 코루틴으로 실행**해야 하는 상황에서는, 다이얼로그의 **목적과 복잡도**에 따라 적합한 선택이 달라집니다.

---

## ⚖️ 상황별 추천 및 선택 가이드

|**기준**|**if (showDialog) (일반 Composable Dialog)**|**NavController.dialog() (Dialog Destination)**|
|---|---|---|
|**추천 순위**|⭐⭐⭐⭐ (단순하고 빠름)|⭐⭐⭐⭐⭐ (구조적이고 확장성이 좋음)|
|**적합한 상황**|다이얼로그가 **매우 단순한 보조 기능**이거나, `check(param)` 로직의 결과가 **현재 화면에만 직접적으로 영향**을 줄 때.|다이얼로그가 **독립적인 UI/UX**를 가지며, **풀스크린**이거나, `check(param)` 로직이 **광범위한 상태/데이터**를 다룰 때.|
|**상태 관리**|`showDialog` 상태와 `viewModel` 로직이 **같은 Composable 내**에 있어 응집도가 높음.|다이얼로그는 내비게이션으로, 로직은 전용 `ViewModel` 또는 공유 `ViewModel`로 **분리**되어 있어 구조적임.|
|**풀스크린 구현**|`fillMaxSize()` 등으로 **수동 설정**이 필요하며, 애니메이션 구현이 복잡함.|`NavController`가 화면 전환을 담당하여 **구현이 훨씬 깔끔함**.|

---

### 🥇 1순위 추천: `NavController.dialog()` (구조적인 접근)

풀스크린 다이얼로그이고 비즈니스 로직(ViewModel 호출)이 들어간다면, **`NavController.dialog()`를 사용하는 것이 장기적으로 가장 좋습니다.**

#### **장점:**

1. **명확한 역할 분리:** 다이얼로그를 독립적인 화면(Destination)으로 취급하여, 다이얼로그 내부 로직과 UI를 메인 화면에서 완전히 분리할 수 있습니다.
    
2. **쉬운 내비게이션 관리:** 뒤로 가기 처리, 생명주기 관리 등을 Compose Navigation 시스템이 알아서 처리합니다.
    
3. **ViewModel 사용의 권장 방식:** `viewModel.check(param)`과 같은 비즈니스 로직은 다이얼로그의 `onClick` 이벤트에서 처리하고 `popBackStack()`을 호출하는 방식이 구조적으로 가장 깔끔합니다. **코루틴 스코프(Scope)**는 다이얼로그의 `ViewModel` 내부의 `viewModelScope`를 사용하는 것이 Compose 권장 사항에 더 부합합니다.
    

### 🥈 2순위 추천: `if (showDialog)` (간단하고 빠른 구현)

만약 다이얼로그가 풀스크린이라도 그 역할이 **매우 제한적**이고 **메인 화면의 컨텍스트를 벗어나지 않는다면**, 이 방법이 더 빠르고 간단합니다.

#### **장점:**

1. **구현의 단순성:** `onDismissRequest` 콜백 한 곳에서 상태 변경과 로직 호출이 동시에 일어나기 때문에 코드를 파악하기 쉽습니다.
    
2. **Scope 관리:** 이미 현재 화면에서 확보된 `rememberCoroutineScope()`를 사용할 수 있습니다.
    

---

### 💡 최종 결론

**풀스크린** 다이얼로그라면, 그것은 이미 단순한 알림 창이 아니라 **'화면' 역할**을 한다는 의미입니다. 따라서 **구조적이고 확장성**이 좋은 **`NavController.dialog()`** 방식을 사용하는 것을 **강력하게 추천**합니다.

다만, 코루틴 로직(`viewModel.check(param)`)은 다이얼로그 닫기 버튼의 `onClick` 이벤트에 연결하고 `navController.popBackStack()`을 그 다음에 호출하는 방식으로 변경해야 합니다.