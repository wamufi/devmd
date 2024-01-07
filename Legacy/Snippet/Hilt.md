Hilt
---
Cache.kt
```kotlin
@Singleton
class Cache @Inject constructor(private val prefs: AppSharedPreferences): BaseCache<String>(500) {
    fun test() {
        val prefsValue = prefs[PrefKey.TEST] ?: true
    }
}
```

Utils.kt
```kotlin
class Utils @Inject constructor() {
    fun test() {
        println("yes test")
    }
}
```

Fragment.kt
```kotlin
class Fragment @Inject constructor(private val cache: Cache): Fragment() {
    @Inject lateinit var utils: Utils

    fun test() {
        cache.test()
        utils.test()
    }
}
```
@Singleton 으로 로그 찍으면 주소값 동일

https://developer88.tistory.com/349


inject 말고 그냥 파라미터 사용 시에는 @AssistedInject를 @Inject 대신 사용, 파라미터에는 @Assisted 사용, @AssistedFactory 인터페이스 생성


변수로 singleton 가져올 때
---
UseCase.kt
```kotlin
operator fun invoke() {
    val preferenceInterface = EntryPointAccessors.fromApplication(WhoWhoApp.instance.applicationContext, PreferenceInterface::class.java)
    val prefs = preferenceInterface.getAppSharedPreference()
}
```

di > entrypoint > PreferenceInterface.kt
```kotlin
@EntryPoint
@InstallIn(SingletonComponent::class)
interface PreferenceInterface {
    fun getAppSharedPreference(): AppSharedPreferences
}
```

di > module > DataModule.kt
```kotlin
@Module
@InstallIn(SingletonComponent::class)
class DataModule {
    @Singleton
    @Provides
    fun providePreference(@ApplicationContext context: Context): AppSharedPreferences {
        return AppSharedPreferences(context, AppSharedPreferences.DATA_FILE_NAME)
    }
}
```

AppSharedPreferences.kt
```kotlin
class AppSharedPreferences(
    context: Context,
    preferenceFileName: String
) {
    val sharedPreferences: SharedPreferences = context.applicationContext.getSharedPreferences(preferenceFileName, Context.MODE_PRIVATE)

    fun clearAll() {
        sharedPreferences.edit().clear().apply()
    }

    fun isExistPrefKey(key: String): Boolean {
        return sharedPreferences.contains(key)
    }

    /**
     * val value: T = preferences[key]
     * val value: T = preferences[key, false]
     */
    inline operator fun <reified T: Any> get(
        key: String,
        defaultValue: T? = null,
        type: Class<T>? = null
    ): T? {
        return when(type ?: T::class.java) {
            java.lang.String::class.java -> {
                sharedPreferences.getString(key, defaultValue as? String) as? T
            }

            java.lang.Integer::class.java -> {
                sharedPreferences.getInt(key, defaultValue as? Int ?: -1) as? T
            }

            java.lang.Long::class.java -> {
                sharedPreferences.getLong(key, defaultValue as? Long ?: -1L) as? T
            }

            java.lang.Float::class.java -> {
                sharedPreferences.getFloat(key, defaultValue as? Float ?: -1f) as? T
            }

            java.lang.Boolean::class.java -> {
                sharedPreferences.getBoolean(key, defaultValue as? Boolean ?: false) as? T
            }

            Set::class.java -> {
                sharedPreferences.getStringSet(key, defaultValue as? Set<String>) as? T
            }

            else -> {
                getTypedObject(key, defaultValue, type ?: T::class.java)
            }
        }
    }

    inline fun <reified T : Any> getTypedObject(
        key: String,
        defValue: T?,
        type: Class<T>
    ): T? =
        try {
            sharedPreferences.getString(key, null)?.let { value ->
                Moshi.Builder().add(MoshiArrayListJsonAdapter.FACTORY).build().adapter(type).fromJson(value)
            } ?: defValue
        } catch (e: com.squareup.moshi.JsonDataException) {
            LogHelper.d("Json parsing failed. Remove data for $key.")
            set(key, null)
            null
        } catch (e: Throwable) {
            defValue ?: throw IllegalArgumentException()
        }

    /**
     * preferences[key] = value
     * preferences[key] = null
     */
    @Synchronized
    operator fun set(key: String, value: Any?) = when (value) {
        null -> {
            edit { it.remove(key) }
        }

        is String -> {
            edit { it.putString(key, value) }
        }

        is Int -> {
            edit { it.putInt(key, value) }
        }

        is Long -> {
            edit { it.putLong(key, value) }
        }

        is Float -> {
            edit { it.putFloat(key, value) }
        }

        is Boolean -> {
            edit { it.putBoolean(key, value) }
        }

        is Set<*> -> {
            putStringSet(key, value)
        }

        else -> {
            putJsonString(key, value)
        }
    }

    private inline fun edit(action: (SharedPreferences.Editor) -> Unit) = sharedPreferences.edit().also(action).apply()

    private fun putJsonString(key: String, value: Any) =
        try {
            edit {
                it.putString(
                    key,
                    Moshi.Builder().add(MoshiArrayListJsonAdapter.FACTORY).build()
                        .adapter(value.javaClass).toJson(value)
                )
            }
        } catch (e: Throwable) {
            LogHelper.d("[Exception]: ${e.message}")
            throw IllegalArgumentException()
        }

    private fun putStringSet(key: String, value: Set<*>) {
        val isValidValue: Boolean = value.count { it !is String } == 0
        if (!isValidValue) {
            return
        }
        edit {
            @Suppress("UNCHECKED_CAST")
            it.putStringSet(key, value as? Set<String>)
        }
    }

    companion object {
        const val DATA_FILE_NAME = "com.ktcs.whowho2"
    }
}
```
// TODO: DBUtils(인터페이스 아닌 사용되는 클래스)도 확인해보고 추가 예정

Android 클래스(ex. Activity, Service, Fragment etc.)가 아닐 때 Hilt 사용
---
PhoneListener.kt (사용하고자 하는 클래스)
```kotlin
class PhoneListener : PhoneStateListener() {
    private val topScreenManagerListener = EntryPointAccessors.fromApplication(WhoWhoApp.instance.applicationContext, ListenerInterface::class.java).getTopScreenManagerListener() // EntryPoint의 함수 불러오기
}
```
ListenerInterface.kt (EntryPoint)
```kotlin
@EntryPoint
@InstallIn(SingletonComponent::class)
interface ListenerInterface {
    fun getTopScreenManagerListener(): TopScreenManagerListener
}
```

DataModule.kt (Module) // 없어도 되는지 확인 필요
```kotlin
@Module
@InstallIn(SingletonComponent::class)
class DataModule {
	@Singleton
	@Provides
	fun provideTopScreenManager(): TopScreenManagerListener {
		return TopScreenManager()
	}
}
```

TopScreenManagerListener.kt (사용되는 인터페이스)
```kotlin
interface TopScreenManagerListener {
    fun startServiceRingingCall(context: Context, number: String, requestCode: Int? = null)
}
```

TopScreenManager.kt (사용되는 인터페이스 구현 클래스)
```kotlin
@AndroidEntryPoint
class TopScreenManager : Service(), TopScreenManagerListener {
    override fun startServiceRingingCall(context: Context, number: String, requestCode: Int? = null) {

    }
}
```

Android 클래스(ex. Activity, Service, Fragment etc.)일 때 Hilt 사용
---
CallReceiver.kt (사용하고자 하는 클래스)
```kotlin
@AndroidEntryPoint
class CallReceiver : BroadcastReceiver() {
    @Inject
    lateinit var topScreenManagerListener: TopScreenManagerListener
}
```

DataModule.kt (Module)
```kotlin
@Module
@InstallIn(SingletonComponent::class)
class DataModule {
	@Singleton
	@Provides
	fun provideTopScreenManager(): TopScreenManagerListener {
		return TopScreenManager()
	}
}
```

TopScreenManagerListener.kt (사용되는 인터페이스)
```kotlin
interface TopScreenManagerListener {
    fun startServiceRingingCall(context: Context, number: String, requestCode: Int? = null)
}
```

TopScreenManager.kt (사용되는 인터페이스 구현 클래스)
```kotlin
@AndroidEntryPoint
class TopScreenManager : Service(), TopScreenManagerListener {
    override fun startServiceRingingCall(context: Context, number: String, requestCode: Int? = null) {

    }
}