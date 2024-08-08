---
title: Material Design 3 in Compose
date: 2024-08-08
time: 11:26:10
tags:
- android
- kotlin
- compose
---
ComposeTheme.kt
```
@Composable  
fun ComposeTheme(darkTheme: Boolean = isSystemInDarkTheme(), content: @Composable () -> Unit) {  
    val dynamicColor = Build.VERSION.SDK_INT >= Build.VERSION_CODES.S  
    val colors = when {  
        dynamicColor && darkTheme -> dynamicDarkColorScheme(LocalContext.current)  
        dynamicColor && !darkTheme -> dynamicLightColorScheme(LocalContext.current)  
        darkTheme -> darkColorScheme()  
        else -> lightColorScheme()  
    }  
    MaterialTheme(colorScheme = colors, content = content)  
}
```

MainScreen.kt
```
@Composable
fun MainScreen() {
	ComposeTheme {
		// ...
	}
}
```

https://developer.android.com/develop/ui/compose/designsystems/material3