---
boostnote:
  createdAt: '2022-03-30T15:04:57.548Z'
  updatedAt: '2022-03-30T15:09:15.193Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Material 3 디자인 적용
  tags:
    - Android
    - Kotlin
    - Material3
  linesHighlighted: []
  isStarred: false
  key: 78af0cf1-7e75-4f1e-976c-ead8eac46e15
  storage: d112f8ec1e85e056a09d
---

Material 3 디자인 적용
---
value/themes.xml
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.AirPollution" parent="Theme.Material3.DynamicColors.DayNight">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
<!--        <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>-->
<!--        <item name="android:windowTranslucentStatus">true</item>-->

        <!-- NoActionBar -->
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>

    <style name="Theme.AirPollution" parent="Base.Theme.AirPollution" />

</resources>
```
DynamicColors에는 NoActionBar가 없기 때문에 수동으로 정의한다.

value-29/themes.xml
```kotlin
<resources>
    <style name="Theme.AirPollution" parent="Base.Theme.AirPollution">
        <!-- Transparent system bars for edge-to-edge. -->
        <item name="android:navigationBarColor">@android:color/transparent</item>
        <item name="android:statusBarColor">@android:color/transparent</item>
        <item name="android:windowLightStatusBar">?attr/isLightTheme</item>
    </style>

</resources>
```

MainActivity.kt
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    WindowCompat.setDecorFitsSystemWindows(window, false) // Material3
    super.onCreate(savedInstanceState)

    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    setSupportActionBar(binding.appBarMain.toolbar) // Material3
}
```

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:openDrawer="start">

    <include
        android:id="@+id/app_bar_main"
        layout="@layout/app_bar_main"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/nav_header_main"
        app:menu="@menu/activity_main_drawer" />
</androidx.drawerlayout.widget.DrawerLayout>
```
Drawer 존재 시에만 DrawerLayout 필요


app_bar_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:fitsSystemWindows="true">

        <com.google.android.material.appbar.MaterialToolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize" />

    </com.google.android.material.appbar.AppBarLayout>

    <include layout="@layout/content_main" />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_marginEnd="@dimen/fab_margin"
        android:layout_marginBottom="16dp"
        app:srcCompat="@android:drawable/ic_dialog_email" />

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```
fitsSystemWindows에 주목
안 하면 statusBar와 navigationBar가 깔끔하게 나오지 않는다.