---
boostnote:
  createdAt: '2022-03-25T05:48:54.409Z'
  updatedAt: '2022-03-25T05:50:45.085Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: '?attr/ 코드에서 사용하기'
  tags:
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: a1535056-9a96-4202-bb97-bc02cfdd5b28
  storage: d112f8ec1e85e056a09d
---

?attr/ 코드에서 사용하기
---
`android:background="?android:attr/colorOnBackground"`를 코드에서 사용해보자

```kotlin
val value = TypedValue()
context.theme.resolveAttribute(R.attr.colorOnBackground, value, true)
setBackgroundColor(value.data)
```

색상값은 여기서
https://m3.material.io/styles/color/the-color-system/tokens
