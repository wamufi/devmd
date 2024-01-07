---
boostnote:
  createdAt: '2022-03-10T03:18:31.770Z'
  updatedAt: '2022-03-10T03:20:19.821Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: RecyclerView 아이템 사이 간격
  tags:
    - Kotlin
    - Android
    - RecyclerView
  linesHighlighted: []
  isStarred: false
  key: 72af9012-cb93-4eba-bb82-fa3ef30cf3b4
  storage: d112f8ec1e85e056a09d
---

RecyclerView 아이템 사이 간격
---
```kotlin
class SpacesItemDecoration(private val space: Int) : ItemDecoration() {
    override fun getItemOffsets(outRect: Rect, view: View, parent: RecyclerView, state: RecyclerView.State) {
        outRect.left = space
        outRect.right = space
        outRect.bottom = space

        // Add top margin only for the first item to avoid double space between items
        // 첫 아이템일 경우 상단 space
//        if (parent.getChildLayoutPosition(view) == 0) {
//            outRect.top = space
//        } else {
//            outRect.top = 0
//        }
    }
}
```

사용은
```kotlin
recyclerView.addItemDecoration(SpacesItemDecoration(15))
```