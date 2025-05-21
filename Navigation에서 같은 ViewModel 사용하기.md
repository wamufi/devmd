---
title: 같은 ViewModel 사용하기
date:  2025-05-21
time:  05:45:40
tags:
  - android
  - compose
  - kotlin
---
FirstScreen과 SecondScreen에서 같은 ViewModel 인스턴스 사용하기
```kotlin
fun NavGraphBuilder.firstSection(navController: NavController) {  
    navigation<FirstRoute.Base>(startDestination = BibleRoute.Bible) {  
        composable<FirstRoute.First> {  
            FirstScreen(onClick = { index ->  
                navController.navigate(route = FirstRoute.Second(index))  
            })  
        }  
  
        composable<FirstRoute.Second>() { backStackEntry ->  
            val parentEntry = navController.getBackStackEntry(route = FirstRoute.First)  
//            val parentEntry = remember(backStackEntry) { navController.getBackStackEntry(route = FirstRoute.First) }  
            val viewModel: FirstViewModel = hiltViewModel(parentEntry)  
  
            val second: FirstRoute.Second = backStackEntry.toRoute()  
  
            SecondScreen(viewModel, second.index, onBack = { navController.popBackStack() }, onClick = {})  
        }  
    }
}
```

