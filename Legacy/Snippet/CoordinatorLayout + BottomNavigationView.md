---
boostnote:
  createdAt: '2022-03-02T05:15:21.445Z'
  updatedAt: '2022-03-14T06:27:20.996Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: CoordinatorLayout + BottomNavigationView
  tags:
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: 30f70391-e23f-43c6-993c-ec9c27ced649
  storage: d112f8ec1e85e056a09d
---

CoordinatorLayout + BottomNavigationView
---
CoordinatorLayout + BottomNavigationView 그리고 스크롤 되는 fragment 사용 시에 framgent랑 bottom navigation view 겹치는 거

물리 키, 소프트 키, pixel 시리즈 하단 홈버튼 다 잘 작동

```kotlin
class ScrollingViewWithBottomNavigationBehavior(context: Context, attrs: AttributeSet) : AppBarLayout.ScrollingViewBehavior(context, attrs) {
    private var bottomMargin = 0

    override fun layoutDependsOn(parent: CoordinatorLayout, child: View, dependency: View): Boolean {
        return super.layoutDependsOn(parent, child, dependency) || dependency is BottomNavigationView
    }

    override fun onDependentViewChanged(parent: CoordinatorLayout, child: View, dependency: View): Boolean {
        val result = super.onDependentViewChanged(parent, child, dependency)

        return if(dependency is BottomNavigationView && dependency.height != bottomMargin) {
            bottomMargin = dependency.height
            val layout = child.layoutParams as CoordinatorLayout.LayoutParams
            layout.bottomMargin = bottomMargin
            child.requestLayout()
            true
        } else {
            result
        }
    }
}

```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_above="@id/nav_view"
    android:fitsSystemWindows="true">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:fitsSystemWindows="true">

        <com.google.android.material.appbar.MaterialToolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize" />

    </com.google.android.material.appbar.AppBarLayout>

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/nav_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:fitsSystemWindows="true"
        app:menu="@menu/bottom_nav_menu" />

    <fragment
        android:id="@+id/nav_host_fragment_activity_main"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:layout_behavior=".ScrollingViewWithBottomNavigationBehavior"
        app:navGraph="@navigation/mobile_navigation" />


</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

https://stackoverflow.com/questions/47917354/coordinatorlayout-content-child-overlaps-bottomnavigationview
