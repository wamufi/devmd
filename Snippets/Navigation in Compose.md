---
title: Navigation in Compose
date:  2025-04-09
time:  03:08:16
tags:
  - android
  - kotlin
  - compose
---
NavGraph.kt
```kotlin
NavHost(navController = navController, startDestination = "a") {
    composable("a") { AScreen(navController) }
    composable("d/{param}") { backStackEntry ->
        val param = backStackEntry.arguments?.getString("param")
        DScreen(param)
    }
}
```

AScreen.kt
```kotlin
@Composable
fun AScreen(navController: NavController) {
    val param = "example"

    BComposable(
        onCardClick = {
            navController.navigate("d/$param")
        }
    )
}
```

BComposable
```kotlin
@Composable
fun BComposable(onCardClick: () -> Unit) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .clickable { onCardClick() }
    ) {
        Text("Go to D")
    }
}
```

- **Navigation 관련 로직은 NavController가 있는 컴포저블에 작성**해야 하며,
- **하위 컴포저블에는 콜백(`onCardClick`) 형태로 넘기는 게 일반적**이에요.
- 이런 방식은 Compose에서 **단방향 데이터 흐름**(unidirectional flow)을 잘 지키는 형태고, 유지보수도 쉬워요.

"NavController를 위->아래로 전달하는 것은 복잡하다."
- **단방향 데이터 흐름(UDF)**: 데이터나 동작은 위에서 아래로 전달. 이건 컴포즈의 핵심 패러다임 중 하나야.
- **테스트 용이성**: 콜백으로 넘기면 테스트하거나 UI 상태 추적이 쉬워짐.
- **재사용성**: BComposable은 `NavController`에 의존하지 않으니까, **다른 화면에서도 재사용 가능**.

1. NavController를 Local로 제공
```kotlin
val navController = rememberNavController()

CompositionLocalProvider(LocalNavController provides navController) {
    NavHost(...)
}

// BComposable
val navController = LocalNavController.current
navController.navigate("d/$param")
```
단, 공식적으로 `LocalNavController`는 없기 때문에 **직접 만들거나 DI에 의존**해야 돼. (Hilt나 Koin 사용 시 싱글톤으로 처리할 수도 있고.)

2. ViewModel로 이벤트 위임
```kotlin
// ViewModel
sealed class UiEvent {
    data class NavigateToD(val param: String) : UiEvent()
}

// AScreen
LaunchedEffect(Unit) {
    viewModel.uiEvent.collect { event ->
        when (event) {
            is UiEvent.NavigateToD -> navController.navigate("d/${event.param}")
        }
    }
}
```

- `NavController`를 콜백으로 전달하는 방식이 **Compose의 권장 방식**이고,
- 복잡할 땐 **ViewModel 이벤트 기반 처리** 또는 **CompositionLocal 커스텀 제공**도 고려해볼 수 있어.
- 단, "다 넘기기 귀찮으니까 그냥 어디서나 NavController 쓰자"는 안드로이드 Compose의 정신과는 좀 거리가 있어 😅