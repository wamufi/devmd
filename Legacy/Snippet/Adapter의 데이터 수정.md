---
boostnote:
  createdAt: '2022-03-20T09:15:35.506Z'
  updatedAt: '2022-03-23T09:54:25.935Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Adapter의 데이터 수정
  tags:
    - Android
    - Kotlin
    - Adapter
  linesHighlighted: []
  isStarred: false
  key: 7b25e367-454e-450a-b7fb-49485257fce7
  storage: d112f8ec1e85e056a09d
---

Adapter의 데이터 수정
---
`notifyDataSetChanged()` 계열 친구들은 adapter class 내에서 제대로 작동하는 것 같다.

어댑터 아이템 삭제 기능을 만들었는데 제대로 보여지지가 않았다.
처음엔 fragment 내에서 viewmodel의 remove 함수를 불러와 코딩하였으나 어댑터에 반영이 제대로 되지 않았고 UI 상으로 요상하게 작동되었다.
그래서 어댑터 내에서 viewmodel의 remove 함수를 불러오기로.
단, dataSet은 MutableList여야 한다.

LiveData를 활용한 다른 방법이 있지 않을까 싶은데 잘 모르겠다.
[[MVVM] 완전 쉬운 RecyclerView에 LiveData 적용예제 + 설명(DataBinding, ViewModel도 사용)](https://gwynn.tistory.com/59)
이렇게? 아직 시도는 안 해봤다.

Adapter.kt
```kotlin
fun removeData(position: Int) {
    val viewModel = ViewModelProvider(context as FragmentActivity)[HmDBViewModel::class.java]
    viewModel.myFixNumRemove(dataSet[position].fixSeq.toString())

    dataSet.removeAt(position)
    notifyItemRemoved(position)
}
```

[[안드로이드] recyclerview drag and drop/swipe 기능 만들기](https://min-wachya.tistory.com/171)