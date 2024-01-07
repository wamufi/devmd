---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: ScrollView 안에 RecyclerView 넣을 때
  tags:
    - Android
  isStarred: false
  createdAt: '2018-04-12T11:47:30.943Z'
  updatedAt: '2019-03-06T01:33:44.705Z'
  linesHighlighted: []
  key: ef61e408e7b67f8f45cc
  storage: d112f8ec1e85e056a09d
---

ScrollView 안에 RecyclerView 넣을 때
---
```xml
<android.support.v4.widget.NestedScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
     <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">
    
        <android.support.v7.widget.RecyclerView
                android:id="@+id/recyclerview"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:nestedScrollingEnabled="false" />

    </LinearLayout>

</android.support.v4.widget.NestedScrollView>
```