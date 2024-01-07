---
boostnote:
  createdAt: '2020-01-02T07:27:55.284Z'
  updatedAt: '2020-01-03T03:19:12.299Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: TextView Scrollable
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: 3b7bbbc9-f7a8-4089-b561-668f2e50919b
  storage: d112f8ec1e85e056a09d
---

TextView Scrollable
---
```java
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true"
    android:scrollbars="vertical">

    <TextView
        android:id="@+id/policy_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="18dp"
        android:layout_marginTop="18dp"
        android:layout_marginEnd="18dp"
        android:layout_marginBottom="18dp" />

</ScrollView>
```