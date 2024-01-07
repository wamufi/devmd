---
boostnote:
  createdAt: '2021-04-09T07:09:18.638Z'
  updatedAt: '2022-02-13T11:16:03.437Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Retrofit + OkHttp JSON
  tags:
    - Retrofit
    - OkHttp
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: 3eaed05a-09cd-45b3-862f-f4be5a885a46
  storage: d112f8ec1e85e056a09d
---

Retrofit + OkHttp JSON
---
자주 흔히 많이 쓰는 두 개의 조합 사용

사용 Api는 GET 방식의 json (공공데이터)
```json
{"recTime":1617951447,"reqId":"GOA002","status":200,"result":{"data":{"mTime":1617983755,"pm2.5":"0","pm10":"0","co":"0.268","so2":"0.001","no2":"0.008","temp":"19.0","hum":"23.6","wd":"NE","ws":"1.3","status":"1"}}}
```

app/build.gradle
```
dependencies {
    implementation 'com.squareup.retrofit2:retrofit:2.5.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.5.0'
    implementation 'com.squareup.okhttp3:okhttp:3.12.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:3.4.2'
}
```

TestApiInterface.kt
```kotlin
interface TestApiInterface {
    @GET("/6410000/GOA/GOA002")
    fun getTestData(
            @Query("busno") busno: String
    ): Call<TestData>

    companion object {
        private const val BASE_URL = "http://apis.data.go.kr/"
        private const val serviceKey = "sL41G3LEKJpMlhZfXSusoNdsU9k4R0ncCw5T8Kp5b6prMBaF8okef2cUhcPaTVS4jXrAihRq6J3%2Bpyj9nNkaZw%3D%3D"

        fun create(): TestApiInterface {
            // OkHttp 로깅
            val httpLoggingInterceptor = HttpLoggingInterceptor()
            httpLoggingInterceptor.level = HttpLoggingInterceptor.Level.BODY

            // 서비스 키
            // 항상 @Query("serviceKey") serviceKey: String 으로 할 수 없으니까
            // class KeyInterceptor(): Interceptor 따로 클래스를 만들어서 사용도 가능
            // 그럴 때는 하단 코드와 동일
            val keyInterceptor = Interceptor {
                val url = it.request().url().newBuilder().addEncodedQueryParameter("serviceKey", serviceKey).build()
                val request = it.request().newBuilder().url(url)
                return@Interceptor it.proceed(request.build())
            }
            
            val client = OkHttpClient.Builder().addInterceptor(httpLoggingInterceptor).addInterceptor(keyInterceptor).build()

            return Retrofit.Builder().baseUrl(BASE_URL).client(client).addConverterFactory(GsonConverterFactory.create()).build().create(TestApiInterface::class.java)
        }
    }
}
   
```

TestData.kt
```kotlin
data class TestData(
    var reqTime: Int = 0,
    var reqId: String = "",
    var result: Results
) {
    data class Results(
        var data: Datas
    ) {
        data class Datas(
            var mTime: Int = 0,
            @SerializedName("pm2.5") var pm25: String = "", // json의 key와 변수 값이 다를 때 @SerializedName(json_key) 사용
            var pm10: String = ""
        )
    }
}
```

MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {
    private val testApi = TestApiInterface.create()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        testApi.getTestData("23537").enqueue(object : Callback<TestData> {
            override fun onResponse(call: Call<TestData>, response: Response<TestData>) {
                Log.v("api", "success: " + response.body())
            }

            override fun onFailure(call: Call<TestData>, t: Throwable) {
                Log.v("api", "failure: " + t.localizedMessage)
            }
        })
    }
}
```

[Android - Retrofit2, OkHttp를 함께 사용하여 더욱 간결한 RestAPI 연동 (Kotlin) (2) \| 잡다한 IT 개발 이야기](https://hwanine.github.io/android/Retrofit-OkHttp/)
[Retrofit2 + OkHttp3 사용하기 - 92Hz](https://jongmin92.github.io/2018/01/29/Programming/android-retrofit2-okhttp3/)
[Android - Retrofit2, OkHttp를 함께 사용하여 더욱 간결한 RestAPI 연동 (Kotlin) (2) \| 잡다한 IT 개발 이야기](https://hwanine.github.io/android/Retrofit-OkHttp/)