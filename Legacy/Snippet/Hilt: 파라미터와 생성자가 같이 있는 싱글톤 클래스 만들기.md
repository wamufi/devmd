Hilt: 파라미터와 생성자가 같이 있는 싱글톤 클래스 만들기
---
클래스
```kotlin
class DeleteRecentCache @AssistedInject constructor (private val isDeleteRecentUseCase: IsDeleteRecentUseCase, private val getDeleteRecentsUseCase: GetDeleteRecentsUseCase, @Assisted size: Int) : BaseCache<Boolean>() {

    @AssistedFactory
    interface DeleteRecentCacheFactory {
        fun create(size: Int): DeleteRecentCache // 파라미터: size
    }

    companion object {
        @Volatile
        private var INSTANCE: DeleteRecentCache? = null

        fun getInstance(factory: DeleteRecentCacheFactory): DeleteRecentCache {
            return INSTANCE ?: synchronized(this) {
                factory.create(1500).also {
                    INSTANCE = it
                }
            }
        }
    }

    /**
     * @param context
     * @param key   {TYPE}_{CONTET_ID}
     * @return
     */
    // 동기 사용시 UI랙 발생
    fun getValue(context: Context, key: String): Boolean {
        if (key.isEmpty()) return false

        var cacheValue = getValue(key)
        if (cacheValue == null) {
            val keys = key.split("_")
            if (keys.size > 1) {
                CoroutineScope(Dispatchers.IO).launch {
                    cacheValue = isDeleteRecentUseCase(keys[1], keys[0]) > 0
                    setValue(key, cacheValue ?: false)
                }
            }
        }

        return cacheValue ?: false
    }

    // DB 테이블 조회 체크 Cost 제거를 위해
    fun getHashValueCheck(context: Context, key: String, hasResult: Boolean): Boolean {
        if (key.isEmpty()) return false

        val cacheValue = getValue(key)
        if (cacheValue == null) {
            val keys = key.split("_")
            if (keys.size > 1) {
                setValue(key, hasResult)
            }
        }

        return hasResult
    }

    // 캐시 동기화
    fun syncCache(limit: Int) {
        CoroutineScope(Dispatchers.IO).launch {
            val deleteRecents = getDeleteRecentsUseCase(limit)
            deleteRecents.collectLatest {
                for (key in it) {
                    setValue(key, true)
                }
            }
        }
    }
}
```

사용
```kotlin
@Inject
lateinit var deleteRecentCacheFactory: DeleteRecentCache.DeleteRecentCacheFactory

fun useDeleteRecentCache() {
//        val deleteRecentCache = deleteRecentCacheFactory.create(1500)
    val deleteRecentCache = DeleteRecentCache.getInstance(deleteRecentCacheFactory)
    DbLog.d("deleteRecentCache: $deleteRecentCache")
    deleteRecentCache.syncCache(0)
}

fun getDeleteRecentsValue(context: Context) {
    val deleteRecentCache = DeleteRecentCache.getInstance(deleteRecentCacheFactory)
    DbLog.d("deleteRecentCache: $deleteRecentCache")
    deleteRecentCache.getValue(context, "1_12345")
}
```