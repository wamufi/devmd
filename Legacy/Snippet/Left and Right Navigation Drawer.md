---
boostnote:
  createdAt: '2018-10-25T05:29:39.599Z'
  updatedAt: '2019-03-06T01:33:41.950Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Left and Right Navigation Drawer
  tags:
    - Android
  isStarred: false
  linesHighlighted: []
  key: 96292334-7b9f-4a92-8480-aec1e53cd9a0
  storage: d112f8ec1e85e056a09d
---

Left and Right Navigation Drawer
---
```java
// Left Drawer
NavigationView navigationView = (NavigationView) findViewById(R.id.nav_view);
navigationView.setNavigationItemSelectedListener(this);

// Right Drawer
NavigationView navigationViewEnd = (NavigationView) findViewById(R.id.nav_view_end);
navigationViewEnd.setNavigationItemSelectedListener(this);
```

```xml
<!-- Left Drawer -->
<android.support.design.widget.NavigationView
    android:id="@+id/nav_view"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:layout_gravity="start"
    android:fitsSystemWindows="true"
    app:headerLayout="@layout/nav_header_main"
    app:menu="@menu/activity_main_drawer" />

<!-- Right Drawer -->
<android.support.design.widget.NavigationView
    android:id="@+id/nav_view_end"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:layout_gravity="end"
    android:fitsSystemWindows="true"
    app:headerLayout="@layout/nav_header_main"
    app:menu="@menu/activity_main_drawer" />
```