---
boostnote:
  createdAt: '2021-04-12T06:59:41.088Z'
  updatedAt: '2021-04-12T07:11:44.196Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Retrofit + OkHttp XML
  tags:
    - Retrofit
    - OkHttp
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: 9d911024-bc90-45c6-871e-00f82a36e229
  storage: d112f8ec1e85e056a09d
---

Retrofit + OkHttp XML
---

app/build.gradle
```
dependencies {
    // Retrofit
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.okhttp3:okhttp:3.14.9'
    implementation 'com.squareup.okhttp3:logging-interceptor:3.4.2'

    // Xml Parser
    implementation 'com.tickaroo.tikxml:annotation:0.8.13'
    implementation 'com.tickaroo.tikxml:core:0.8.13'
    implementation 'com.tickaroo.tikxml:retrofit-converter:0.8.13'
    kapt 'com.tickaroo.tikxml:processor:0.8.13' // kotlin-kapt 사용
    // annotationProcessor 'com.tickaroo.tikxml:processor:0.8.15' // kotlin-kapt 미사용
}
```

AirKoreaInterface.kt
```kotlin
interface AirKoreaInterface {
    // 측정소별 실시간 측정정보 조회
    @GET("/openapi/services/rest/ArpltnInforInqireSvc/getMsrstnAcctoRltmMesureDnsty")
    fun getRealTimeInfo(
        @Query("numOfRows") numOfRows: String,
        @Query("pageNo") pageNo: String,
        @Query("stationName") stationName: String,
        @Query("dataTerm") dataTerm: String,
        @Query("ver") ver: String
    ): Call<MsrstnAcctoRltmMesureDnsty>

    companion object {
        private const val BASE_URL = "http://openapi.airkorea.or.kr"
//        private const val SERVICE_KEY = "seUGpp3xcB0kAY09ZmZqw8zZB8Jd0tG/R7N1Cipo/D/0HwKemq9efoVd+LT2+7FSfJYVb8wyz4stz90WV6IhEg=="
        private const val SERVICE_KEY = BuildConfig.SERVICE_KEY

        fun create(): AirKoreaInterface {
            val httpLoggingInterceptor = HttpLoggingInterceptor()
            httpLoggingInterceptor.level = HttpLoggingInterceptor.Level.BODY

            val keyInterceptor = Interceptor {
                val url = it.request().url().newBuilder().addQueryParameter("serviceKey", SERVICE_KEY).build()
                val request = it.request().newBuilder().url(url)
                return@Interceptor it.proceed(request.build())
            }

            val client = OkHttpClient.Builder().addInterceptor(httpLoggingInterceptor).addInterceptor(keyInterceptor).build()
            return Retrofit.Builder().baseUrl(BASE_URL).client(client).addConverterFactory(TikXmlConverterFactory.create(TikXml.Builder().exceptionOnUnreadXml(false).build())).build().create(AirKoreaInterface::class.java)
        }
    }
}
```

MsrstnAcctoRltmMesureDnsty.kt
```kotlin
/**
 * 측정소별 실시간 측정정보 조회
 * 측정소명과 측정데이터 기간(일, 한달, 3개월)으로 해당 측정소의 일반항목 측정정보를 제공하는 측정소별 실시간 측정정보조회
 */
@Xml(name = "response")
data class MsrstnAcctoRltmMesureDnsty(
    @Element
    val header: Header,
    @Element
    val body: Body
)

@Xml(name = "header")
data class Header(
    @PropertyElement
    val resultCode: Int,
    @PropertyElement
    val resultMsg: String
)

@Xml(name = "body")
data class Body(
    @Element
    val items: Items,
    @PropertyElement
    val numOfRows: Int,
    @PropertyElement
    val pageNo: Int,
    @PropertyElement
    val totalCount: Int
)

@Xml
data class Items(
    @Element(name = "item")
    val item: List<Item>
)

@Xml
data class Item(
    @PropertyElement
    var dataTime: String?,
    @PropertyElement
    var pm10Value: String?
) {
    constructor(): this(null, null)
}
```

MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {
    private val airKoreaInterface = AirKoreaInterface.create()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        airKoreaInterface.getRealTimeInfo("10", "1", "종로구", "DAILY", "1.3").enqueue(object : Callback<MsrstnAcctoRltmMesureDnsty> {
            override fun onResponse(call: Call<MsrstnAcctoRltmMesureDnsty>, response: Response<MsrstnAcctoRltmMesureDnsty>) {
                Log.v("airkorea", "MsrstnAcctoRltmMesureDnsty: " + response.body())
            }

            override fun onFailure(call: Call<MsrstnAcctoRltmMesureDnsty>, t: Throwable) {
                Log.v("airkorea", "MsrstnAcctoRltmMesureDnsty failure: " + t.localizedMessage)
            }
        })
    }
}
```

[[Android] Retrofit2을 활용시 반환 타입이 XML일 경우 변환 방법](https://bb-library.tistory.com/177)