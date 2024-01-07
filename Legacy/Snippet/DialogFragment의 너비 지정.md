---
boostnote:
  createdAt: '2021-07-10T09:47:14.106Z'
  updatedAt: '2021-07-10T09:48:00.746Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: DialogFragment의 너비 지정
  tags:
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: 0a35aa88-f5fb-4a84-aea2-3af4ef805b29
  storage: d112f8ec1e85e056a09d
---

DialogFragment의 너비 지정
---
```kotlin
override fun onResume() {
    super.onResume()

    // dialog의 너비를 기기의 80%로 지정
    val windowManager = context?.getSystemService(Context.WINDOW_SERVICE) as WindowManager
    val params: ViewGroup.LayoutParams? = dialog?.window?.attributes
    val deviceWidth = windowManager.defaultDisplay.width
    params?.width = (deviceWidth * 0.8).toInt()
    dialog?.window?.attributes = params as WindowManager.LayoutParams
}
```