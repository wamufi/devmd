---
boostnote:
  createdAt: '2022-03-29T06:43:29.961Z'
  updatedAt: '2022-03-29T06:47:31.939Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Proguard 적용 후 Retrofit 통신 시 null
  tags:
    - Android
    - Kotlin
    - Retrofit
    - Proguard
  linesHighlighted: []
  isStarred: false
  key: a2f4717f-cb67-4799-ae07-cf0bf7c794f0
  storage: d112f8ec1e85e056a09d
---

Proguard 적용 후 Retrofit 통신 시 null
---
존나 대삽질

Proguard 적용하면 data class가 null로 들어와서 NullPointException 발생

ViewModel.kt
```kotlin
fun fetchLottoSync(input: String) {
    viewModelScope.launch {
        repository.fetchLottoSync(input).apply {
            if (isSuccessful) {
                body()?.let {
                    _lotto.value = it.lda as? List<SyncLotto.LottoList> // NullPointException 발생
                }
            } else {
                Log.d("setMyWash", "error ${errorBody()}")
            }
        }
    }
}
```

해결 방법은 `@Keep` 사용
```kotlin
@Keep
data class SyncLotto(
    val lda: List<LottoList?>?
) {
    @Keep
    data class LottoList(
        val dno: Int?,
        val date: String?,
        val st1Num: String?,
        val st1Amt: String?,
        val st2Num: String?,
        val st2Amt: String?,
        val st3Num: String?,
        val st3Amt: String?,
        val st4Num: String?,
        val st4Amt: String?,
        val st5Num: String?,
        val st5Amt: String?,
        val n1: Int?,
        val n2: Int?,
        val n3: Int?,
        val n4: Int?,
        val n5: Int?,
        val n6: Int?,
        val n7: Int?
    )
}
```
여기서 SyncLotto에만 Keep을 줬을 때는 에러가 발생했고 LottoList에도 넣어줘야했다. 지금 보니까 애초에 SyncLotto.LottoList인데 당연히 에러나지 싶네.

https://dog-footprint.tistory.com/28
