Singleton in Kotlin
---
```kotlin
companion object {
    @Volatile
    private var INSTANCE: WhoWhoDatabase? = null

    @JvmStatic
    fun getInstance(context: Context): WhoWhoDatabase {
        return INSTANCE ?: synchronized(this) {
            val builder = Room.databaseBuilder(context, WhoWhoDatabase::class.java, DB.NAME)
            builder.addMigrations(*DB.MIGRATIONS) // 데이터베이스 이전
            builder.addCallback(DatabaseCallback()) // 데이터베이스 미리 채우기
//                builder.setQueryCallback(QueryCallback(), Executors.newSingleThreadExecutor()) // SQL Query Debugging

            val instance = builder.build()
            this.INSTANCE = instance
            instance
        }
    }
}
```
```kotlin
@Volatile
private var INSTANCE: DeleteRecentCache? = null

fun getInstance(): DeleteRecentCache {
    return INSTANCE ?: synchronized(this) {
        DeleteRecentCache(1500).also {
            INSTANCE = it
        }
    }
}

class EndUpdatePopupTheme : ForegroundService() {
    companion object {
        @Volatile
        private var INSTANCE: EndUpdatePopupTheme? = null

        fun getInstance() = INSTANCE ?: synchronized(this) {
            INSTANCE ?: EndUpdatePopupTheme().also { INSTANCE = it }
        }
    }
}
```

https://developer.android.com/codelabs/android-room-with-a-view-kotlin?hl=ko#7
https://sonseungha.tistory.com/596
https://jw910911.tistory.com/56
https://sandn.tistory.com/108
https://stackoverflow.com/questions/56825097/synchronized-singleton-in-kotlin
