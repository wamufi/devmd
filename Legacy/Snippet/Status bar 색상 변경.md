---
boostnote:
  createdAt: '2019-12-10T05:40:26.292Z'
  updatedAt: '2021-06-24T02:11:28.020Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Status bar 색상 변경
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: bb3159f4-2df8-4aee-b3a3-0f70ea0e71c4
  storage: d112f8ec1e85e056a09d
---

Status bar 색상 변경
---
투명 + 화이트
```java
Window window = getWindow();
window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS);
window.setStatusBarColor(getResources().getColor(android.R.color.transparent));
window.getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
```

colorPrimary + 블랙
```xml
<style name="AppTheme.LightStatusBar" parent="AppTheme.NoActionBar">
    <item name="android:statusBarColor">@color/colorPrimary</item>
    <item name="android:windowLightStatusBar">true</item>
</style>
```

colorPrimaryDark + 화이트
```xml
<style name="AppTheme.DarkStatusBar" parent="AppTheme.NoActionBar">
    <item name="android:statusBarColor">@color/colorPrimaryDark</item>
    <item name="android:windowLightStatusBar">false</item>
</style>
```

투명 + 블랙
```xml
<!-- Status Bar 투명, Status Bar Text Color 블랙 -->
<style name="TransparentStatusBar" parent="AppTheme.NoActionBar">
    <item name="android:windowTranslucentStatus">true</item>
    <item name="android:windowTranslucentNavigation">true</item>
    <item name="android:windowLightStatusBar" tools:targetApi="m">true</item>
</style>
```