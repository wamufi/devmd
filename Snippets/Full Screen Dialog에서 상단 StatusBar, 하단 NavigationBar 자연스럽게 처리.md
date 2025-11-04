---
title: Full Screen Dialog에서 상단 StatusBar, 하단 NavigationBar 자연스럽게 처리
date:  2025-11-04
time:  16:35:18
tags:
  - kotlin
  - android
  - compose
---
```kotlin
Dialog(onDismissRequest = onDismissRequest,  
    properties = DialogProperties(usePlatformDefaultWidth = false, decorFitsSystemWindows = false)) {  
  
    val view = LocalView.current  
    val dialogWindow = (view.parent as? DialogWindowProvider)?.window  
  
    // DisposableEffect를 사용하여 Dialog가 사라질 때 Window 설정을 클린업할 준비를 합니다.  
    DisposableEffect(dialogWindow) {  
        dialogWindow?.let { window ->  
            // 1. 시스템 바 배경색을 투명하게 설정 (필수)  
            window.statusBarColor = Color.TRANSPARENT  
            window.navigationBarColor = Color.TRANSPARENT  
  
            // 2. Window Manager 플래그를 사용하여 시스템 바를 투명하게 표시하도록 강제합니다.  
            // FLAG_LAYOUT_NO_LIMITS: Window 캔버스를 화면 전체로 확장하여 시스템 바 아래로 그리도록 허용합니다.  
            window.setFlags(  
                WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS,  
                WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS  
            )  
  
            // 3. Edge-to-Edge 모드 플래그를 Dialog Window에 설정합니다.  
            // 이 설정이 시스템 바를 투명하게 렌더링합니다.  
            WindowCompat.setDecorFitsSystemWindows(window, false)  
  
            // 4. 시계 및 아이콘이 보이도록 아이콘 색상 설정 (Activity와 동일하게 설정)  
            // Activity 배경이 밝은 경우 (일반적):  
            WindowCompat.getInsetsController(window, window.decorView).apply {  
                isAppearanceLightStatusBars = true  
                isAppearanceLightNavigationBars = true  
            }  
        }  
        // Dialog가 사라질 때 (onDispose) 수행할 작업이 있다면 여기에 추가 (예: 플래그 원상 복구)  
        onDispose {  
            // 이 Dialog가 사라진 후 Activity의 Window에 영향을 주지 않도록 주의합니다.  
        }  
    }
    
    // Dialog Content
}
```