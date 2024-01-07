Moshi
---
Json to Data class
```kotlin
val moshi = Moshi.Builder().add(KotlinJsonAdapterFactory()).build()
val adapter = moshi.adapter(RecentSeq::class.java)
val recentSeq = adapter.fromJson(result.data.recentSeq)
```

Data class to Json
```kotlin
val moshi = Moshi.Builder().add(KotlinJsonAdapterFactory()).build()
val adapter = moshi.adapter(Tutorial::class.java)
val value = adapter.toJson(tutorial)
```

DTO class
```kotlin
data class SpamCallLiveDTO(
    @EncryptString @Json(name = "searchPhoneNumber") val searchPhoneNumber: String, // 전화번호
    @EncryptString @Json(name = "fakePhoneNumber") val fakePhoneNumber: String? = null, // 가짜전화번호 (SMS에서만 사용되는 것으로 보임)
    @Json(name = "callType") val callType: String, // 전화(P), 문자(M) 구분
    @Json(name = "inOutType") val inOutType: String, // 수발신 구분 (I/O)
    @Json(name = "requestType") val requestType: String // 용도 구분 (0: 알림창, 1: 나머지)
)
```

VO class
```kotlin
data class GuardiansResponse(
    val ret: String,
    val guardians: List<Guardian>
)

data class Guardian(
    val type: String,
    val wardId: Int,
    val guardianId: Int,
    val guardianEmail: String,
    val guardianPhone: String,
    val agreeStatus: String,
    val sendSms: String,
    val totalPage: Int,
    val totalCount: Int,
    val lastPage: Boolean,
    val callHist: List<CallHistory>
)

data class CallHistory(
    val idx: Int,
    val callHistId: Int,
    val dangerCallType: String,
    val message: String,
    val detailMessage: String,
    val dangerCallDateInMil: Long,
    val poiSearchClose: Boolean,
    val wardPoiSearchList: List<WardPoiSearch>
)

data class WardPoiSearch(
    val idx: Int,
    val poiId: Int,
    @NullString val latTitude: String
)

data class WardRuleResponse(
    @Json(name = "callTime") val callTime: Int,
    @Json(name = "spamTypes") val spamTypes: List<Int>,
    @Json(name = "dangerCallList") val dangerCallList: List<String>,
    @Json(name = "ward") val ward: Boolean
)
```

Retrofit with Hilt
ApiModule.kt
```kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class ApiRetrofit

@Module
@InstallIn(SingletonComponent::class)
class ApiModule {
    @Singleton
    @Provides
    fun provideLoggingInterceptor(): HttpLoggingInterceptor {
        val loggingInterceptor = HttpLoggingInterceptor {
            if (it.startsWith("{")) {
                Log.d("okhttp.OkHttpClient", GsonBuilder().setPrettyPrinting().create().toJson(JsonParser().parse(it)))
            } else {
                Log.d("okhttp.OkHttpClient", it)
            }
        }
        loggingInterceptor.level = if (BuildConfig.DEBUG) {
            HttpLoggingInterceptor.Level.BODY
        } else {
            HttpLoggingInterceptor.Level.NONE
        }
        return loggingInterceptor
    }

    @Singleton
    @Provides
    fun provideHeaderInterceptor(): Interceptor {
        return Interceptor { chain ->
            with(chain) {
                val newRequest = request().newBuilder().build()
                proceed(newRequest)
            }
        }
    }

    @Singleton
    @Provides
    fun provideOkHttpClient(
        headerInterceptor: Interceptor, loggingInterceptor: HttpLoggingInterceptor
    ): OkHttpClient {
        return OkHttpClient().newBuilder().connectTimeout(10, TimeUnit.SECONDS).readTimeout(10, TimeUnit.SECONDS).writeTimeout(10, TimeUnit.SECONDS).addInterceptor(headerInterceptor)
            .addInterceptor(loggingInterceptor).build()
    }

    @Singleton
    @Provides
    fun provideMoshi(): Moshi {
        return Moshi.Builder().apply {
            add(KotlinJsonAdapterFactory())
            JsonAdapterProvider.get().forEach { add(it) }
        }.build()
    }

    @Singleton
    @ApiRetrofit
    @Provides
    fun provideApiRetrofit(okHttpClient: OkHttpClient, moshi: Moshi): Retrofit {
        return Retrofit.Builder().baseUrl(BuildConfig.BASE_URL).client(okHttpClient).addConverterFactory(ScalarsConverterFactory.create()).addConverterFactory(MoshiConverterFactory.create(moshi))
            .addCallAdapterFactory(RxJava2CallAdapterFactory.create()).build()
    }

    @Singleton
    @Provides
    fun provideApiService(@ApiRetrofit retrofit: Retrofit): ApiService {
        return retrofit.create(ApiService::class.java)
    }
}
```

JsonAdapterProvider.kt
```kotlin
object JsonAdapterProvider {

    fun get() = listOf(
        NullToEmptyAdapter(),
        YnBooleanAdapter(),
        StringToEncryptAdapter()
    )
}

@Retention(AnnotationRetention.RUNTIME)
@JsonQualifier
internal annotation class NullString

internal class NullToEmptyAdapter {
	@ToJson
	fun toJson(@NullString value: String): String? {
		return value
	}

	@FromJson
	@NullString
	fun fromJson(value: JsonReader): String {
		if (value.peek() != JsonReader.Token.NULL) {
			return value.nextString()
		}
		value.nextNull<Unit>()
		return ""
	}
}

private const val TRUE = "true"
private const val FALSE = "false"

@Retention(AnnotationRetention.RUNTIME)
@JsonQualifier
internal annotation class YnBoolean

internal class YnBooleanAdapter {
    @ToJson
    fun toJsonNotNull(@YnBoolean value: Boolean) = when (value) {
        true -> TRUE
        else -> FALSE
    }

    @FromJson
    @YnBoolean
    fun fromJsonNotNull(value: String) = when (value) {
        TRUE -> true
        else -> false
    }

    @ToJson
    fun toJsonNullable(@YnBoolean value: Boolean?) = when (value) {
        true -> TRUE
        false -> FALSE
        else -> null
    }

    @FromJson
    @YnBoolean
    fun fromJsonNullable(value: String?) = when (value) {
        TRUE -> true
        FALSE -> false
        else -> null
    }
}

@Retention(AnnotationRetention.RUNTIME)
@JsonQualifier
internal annotation class EncryptString

internal class StringToEncryptAdapter {
    @ToJson
    fun toJson(@EncryptString value: String): String {
        return value.encodeByAES() ?: ""
    }

    @FromJson
    @EncryptString
    fun fromJson(value: String): String {
        return value.encodeByAES() ?: ""
    }
}
```

ApiService.kt
```kotlin
interface ApiService {
    @POST("/whowho_app/v4/advertisement/logs")
	suspend fun fetchADID(
		@Body bodyData: ADidDTO
	): ResultResponse

    // 서버쪽에서 raw 로만 처리하기 때문에 bodyData 를 String 으로 처리 (서버 이슈사항)
	@Headers("Content-Type: application/json")
	@POST("/whowho_app/v4/widget/info")
	suspend fun fetchWidgetInfo(
		@Body bodyData: String
	): WidgetInfoResponse
}
```

DataModule.kt
```kotlin
@Module
@InstallIn(SingletonComponent::class)
class DataModule {
	@Singleton
	@Provides
	fun provideRemoteDataSource(apiService: ApiService, ioDispatcher: CoroutineDispatcher): RemoteDataSource {
		return RemoteDataSourceImpl(apiService, ioDispatcher)
	}

	@Singleton
	@Provides
	fun provideRemoteRepository(
		remoteDataSource: RemoteDataSource,
	): RemoteRepository {
		return RemoteRepositoryImpl(remoteDataSource)
	}
}
```

RemoteDataSource.kt
```kotlin
interface RemoteDataSource {
    fun fetchADID(data: ADidDTO): Flow<DataResult<ResultResponse>>
}

class RemoteDataSourceImpl @Inject constructor(
    private val apiService: ApiService,
    private val ioDispatcher: CoroutineDispatcher
) : RemoteDataSource {
    override fun fetchADID(data: ADidDTO): Flow<DataResult<ResultResponse>> {
		return mediateFlow {
			apiService.fetchADID(data)
		}.flowOn(ioDispatcher)
	}
}
```

DataResult.kt
```kotlin
sealed class DataResult<out T> {
    data class Success<out T>(val data: T): DataResult<T>()
    data class Error<out T>(val failure: Failure): DataResult<T>()
    object Empty: DataResult<Nothing>()
}

fun <T> DataResult<T>.isSuccess(block: (T) -> Unit): DataResult<T> {
    return apply {
        if (this@isSuccess is DataResult.Success) block.invoke(data)
    }
}

fun <T> DataResult<T>.isError(block: (Failure) -> Unit): DataResult<T> {
    return apply {
        if (this@isError is DataResult.Error) block.invoke(failure)
    }
}

fun <T> mediateFlow(apiFunction: suspend () -> T): Flow<DataResult<T>> = flow {
    try {
        emit(DataResult.Success(apiFunction.invoke()))
    } catch (e: HttpException) {
        emit(DataResult.Error(Failure.HttpError(e.code(), e.toString())))
    } catch (e: SocketTimeoutException) {
        emit(DataResult.Error(Failure.HttpError(408, e.message ?: "SocketTimeout")))
    } catch (e: ConnectException) {
        emit(DataResult.Error(Failure.NoConnection))
    } catch (e: UnknownHostException) {
        emit(DataResult.Error(Failure.NoConnection))
    } catch (e: NullPointerException) {
        emit(DataResult.Empty)
    } catch (e: IOException) {
        emit(DataResult.Error(Failure.Exception(Throwable(e.message))))
    } catch (t: Throwable) {
        emit(DataResult.Error(Failure.Exception(t)))
    }
}
```

RemoteRepository.kt
```kotlin
interface RemoteRepository {
    fun fetchADID(data: ADidDTO): Flow<DataResult<ResultResponse>>
}

class RemoteRepositoryImpl @Inject constructor(
	private val remoteDataSource: RemoteDataSource
) : RemoteRepository {

	override fun fetchADID(data: ADidDTO): Flow<DataResult<ResultResponse>> {
		return remoteDataSource.fetchADID(data).map {
			it.also { result ->
				if (result is DataResult.Success) {
					return@map DataResult.Success(result.data)
				}
			}
		}.flowOn(Dispatchers.Default)
	}
}
```

ADIDUseCase.kt
```kotlin
class ADIDUseCase @Inject constructor(
    private val repository: RemoteRepository
) {
    operator fun invoke(data: ADidDTO): Flow<DataResult<ResultResponse>> {
        return repository.fetchADID(data)
    }
}
```