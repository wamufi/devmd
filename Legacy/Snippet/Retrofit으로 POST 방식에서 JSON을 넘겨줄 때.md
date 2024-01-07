---
boostnote:
  createdAt: '2021-04-12T09:10:11.450Z'
  updatedAt: '2021-04-12T09:13:23.445Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Retrofit으로 POST 방식에서 JSON을 넘겨줄 때
  tags:
    - Retrofit
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: c042d282-837c-4b80-9bc6-997282bb0fa4
  storage: d112f8ec1e85e056a09d
---

Retrofit으로 POST 방식에서 JSON을 넘겨줄 때
---
JSON body를 넘겨줄 때

AdTransInfoModel.kt
```kotlin
data class AdTransInfoRequest (
    var reqDttm: String,
    var areaCode: String,
    var vhclIdfKey: String,
    var sig: String
)
```

ApiInterface.kt
```kotlin
@POST(AD_000001)
fun getAdTransInfo(@Body request: AdTransInfoRequest): Call<AdTransInfo>
```

MainActivity.kt
```kotlin
private fun getAdTransInfo() {
    val date = InCarAdUtils().convertDateToString(Date())
    val areaCode = "A01" // 영종도
    val vhclIdfKey = vhclIdfKey
    val sig = InCarAdUtils().getSHA256(date + vhclIdfKey + HASH_KEY)
    val request = sig?.let { AdTransInfoRequest(date, areaCode, vhclIdfKey, it) }

    if (request != null) {
        api.getAdTransInfo(request).enqueue(object : Callback<AdTransInfo> {
            override fun onResponse(call: Call<AdTransInfo>, response: Response<AdTransInfo>) {
                Log.i("api", "AdTransInfo success: " + response.body())
            }

            override fun onFailure(call: Call<AdTransInfo>, t: Throwable) {
                Log.i("api", "AdTransInfo failure: " + t.localizedMessage)
            }
        })
    }
}
```