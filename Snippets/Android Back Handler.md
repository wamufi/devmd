---
title: Android Back Handler
date:  2025-05-12
time:  04:06:17
tags:
  - android
  - kotlin
  - compose
---
시스템 뒤로가기 버튼 구현

### BackHandler
```kotlin
@Composable
fun MyScreen(navController: NavController) {
    BackHandler {
        navController.navigate("Main") {
            popUpTo(navController.graph.id) {
                inclusive = true
            }
        }
    }
}
```

### NavHost에서 공통적으로 처리
```kotlin
import androidx.compose.runtime.Composable
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController

@Composable
fun MainNavGraph() {
    val navController = rememberNavController()

    BackHandler {
        if (navController.currentDestination?.route != "Main") {
            navController.navigate("Main") {
                popUpTo(navController.graph.id) { inclusive = true }
            }
        }
    }

    NavHost(navController = navController, startDestination = "Main") {
        composable("Main") { MainScreen(navController) }
        composable("Detail") { DetailScreen(navController) }
    }
}
```

### 주의사항
* BackHandler는 Composable에서만 사용 가능
* 특정 화면에서만 뒤로가기를 제어하고 싶다면, 해당 Composable 내에서 BackHandler  호출
* navController.popBackStack()을 사용하면 현재 스택에서 하나의 화면만 제거 가능. 이 경우에는, inclusive 옵션(스택에서 제거)이 필요 없음.