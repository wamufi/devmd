SQLite db를 assets에서 복사해서 Room으로 이전
---
MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var job: Job
    private lateinit var db: TestDatabase
    private lateinit var dbHelper: DBHelper

    private val viewModel: SpamCategoryViewModel by viewModels {
        SpamCategoryViewModelFactory((application as WordsApplication).repository)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

//        job = CoroutineScope(Dispatchers.IO).launch {
//            DatabaseCopier.copyAttachedDatabase(applicationContext)
//            db = DatabaseCopier.getDatabase(applicationContext)
//            Log.i("MainActivity", "db: $db")
//        }
//
//        runBlocking {
//            job.join()
//        }
//

        val fab = findViewById<FloatingActionButton>(R.id.fab)
        fab.setOnClickListener {
//                dbHelper.insertSpamCategory()
            viewModel.insert(SpamCategory(null, 55, "ViewModelTEST", 17))
        }

        // insert 시에는 DBHelper로 하면 LiveData로 안 불러옴
        // LiveData는 뷰모델이 없으면 아예 콜을 안함
        viewModel.spamCategories.observe(this) {
            Log.i("MainActivity", "$it")
        }

        // 뷰모델 없이 테스트 용도
//        CoroutineScope(Dispatchers.IO).launch {
//            val database = DatabaseCopier.getDatabase(this)
//            Log.i("MainActivity", database.spamCategoryDao().getSpamCategory())
//        }
    }

    override fun onStart() {
        super.onStart()

        dbHelper = DBHelper(applicationContext)
    }

    override fun onDestroy() {
//        job.cancel()
        super.onDestroy()
    }
}
```

TestApplication.kt
```kotlin
class TestApplication : Application() {
    // 데이터베이스를 채우기 위한 CoroutineScope
    // UI 수명 주기와 관련이 없고 앱의 수명 주기와 관련
    private val applicationScope = CoroutineScope(SupervisorJob())

    val database by lazy { DatabaseCopier.getDatabase(this) }
    val repository by lazy { SpamCategoryRepository(database.spamCategoryDao()) }
}
```

DatabaseCopier.kt
```kotlin
object DatabaseCopier {
    private val TAG = DatabaseCopier::class.java.simpleName
    private const val DATABASE_NAME = "com.ktcs.whowho.db"

    @Volatile
    private var INSTANCE: TestDatabase? = null

    fun getDatabase(context: Context): TestDatabase {
        copyAttachedDatabase(context)

        return INSTANCE ?: synchronized(this) {
            Log.i(TAG, "database null")
            val instance = Room.databaseBuilder(context.applicationContext, TestDatabase::class.java, DATABASE_NAME).addMigrations(TestDatabase.MIGRATE_68_2).build()
            INSTANCE = instance

            instance
        }
    }

    private fun copyAttachedDatabase(context: Context) {
        Log.i(TAG, "copy attached database start")
        val path = context.getDatabasePath(DATABASE_NAME)

        if (path.exists()) {
            Log.i(TAG, "copied db already exists: $path")
            val info = context.packageManager.getPackageInfo(context.packageName, 0)
            val version = PackageInfoCompat.getLongVersionCode(info)

            Log.i(TAG, "package version: $version")

            if (version.toString() != "1") {
                // TODO: 버전 다를 때 처리 지금은 assets에서 가져옴
                Log.i(TAG, "package version is different")
                copyDatabaseFromAssets(context, path)
            }

            return
        }

        Log.i(TAG, "copied db does not exist")

//        path.parentFile?.mkdirs()
//        path.mkdirs()
        copyDatabaseFromAssets(context, path)
    }

    private fun copyDatabaseFromAssets(context: Context, path: File) {
        Log.i(TAG, "copy database from assets start")
        val inputStream = context.assets.open("databases/$DATABASE_NAME")
        val outputStream = FileOutputStream(path)

        try {
            val buffer = ByteArray(8192)
            var length: Int

            while (inputStream.read(buffer).also { length = it } > 0) {
                outputStream.write(buffer, 0, length)
            }

            outputStream.flush()
            outputStream.close()
            inputStream.close()
        } catch (e: IOException) {
            Log.e(TAG, "db copy failure: $e")
            e.printStackTrace()
        }
    }
}
```

TestDatabase.kt
```kotlin
@Database(entities = [SpamCategory::class], version = 2, exportSchema = false)
abstract class TestDatabase : RoomDatabase() {
    abstract fun spamCategoryDao(): SpamCategoryDao

    companion object {
        val MIGRATE_68_2 = object : Migration(68, 2) {
            override fun migrate(database: SupportSQLiteDatabase) {
                Log.i("TestDatabase", "migrate: $database")
//                database.execSQL("ALTER TABLE TBL_SPAM_CATEGORY ")
            }
        }
    }
}
```

SpamCategory.kt
```kotlin
@Entity(tableName = "TBL_SPAM_CATEGORY")
data class SpamCategory(
    @PrimaryKey
    val _ID: Int?,
    @ColumnInfo(name = "CODE")
    val code: Int?,
    @ColumnInfo(name = "NAME")
    val name: String?,
    @ColumnInfo(name = "SEQ")
    val seq: Int?
)
```

SpamCategoryDao.kt
```kotlin
@Dao
interface SpamCategoryDao {
    @Query("SELECT * FROM TBL_SPAM_CATEGORY")
    fun getSpamCategory(): Flow<List<SpamCategory>>
//    fun getSpamCategory(): List<SpamCategory> // 뷰모델 없이 테스트 용도

    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(spamCategory: SpamCategory)
}
```

SpamCategoryRepository.kt
```kotlin
class SpamCategoryRepository(private val spamCategoryDao: SpamCategoryDao) {
    val spamCategories = spamCategoryDao.getSpamCategory()

    suspend fun insert(spamCategory: SpamCategory) {
        spamCategoryDao.insert(spamCategory)
    }
}
```

SpamCategoryViewModel.kt
```kotlin
class SpamCategoryViewModel(private val repository: SpamCategoryRepository) : ViewModel() {
    val spamCategories: LiveData<List<SpamCategory>> = repository.spamCategories.asLiveData()

    fun insert(spamCategory: SpamCategory) = viewModelScope.launch {
        repository.insert(spamCategory)
    }
}

class SpamCategoryViewModelFactory(private val repository: SpamCategoryRepository) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(SpamCategoryViewModel::class.java)) {
            @Suppress("UNCHECKED_CAST")
            return SpamCategoryViewModel(repository) as T
        }

        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```

DBHelper.kt
```kotlin
class DBHelper(context: Context) : SQLiteOpenHelper(context, DATABASE_NAME, null, DATABASE_VERSION) {

    private var db: SQLiteDatabase? = null

    companion object {
        const val DATABASE_NAME = "com.ktcs.whowho.db"
        const val DATABASE_VERSION = 2 // 현재 WhoWho 프로젝트는 68
    }

    private val isDatabaseExists = run {
        try {
            db = SQLiteDatabase.openDatabase(context.getDatabasePath(DATABASE_NAME).path, null, SQLiteDatabase.OPEN_READONLY)
        } catch (e: SQLiteException) {
            e.printStackTrace()
        }

        db?.close()

        db != null
    }

    fun insertSpamCategory() {
        if (isDatabaseExists) {
            db = open()

            Log.i("DBHelper", "db exists: $db")
            val sql = "INSERT INTO TBL_SPAM_CATEGORY(CODE, NAME, SEQ) values('33', 'DBHelperTEST', '15')"
            db?.execSQL(sql)
        } else {
            Log.i("DBHelper", "db does not exist")
        }
    }

    private fun open(): SQLiteDatabase? {
        db?.let {
            if (!it.isOpen) {
                db = writableDatabase
            }
        }

        return db
    }

    override fun onCreate(p0: SQLiteDatabase?) {
        TODO("Not yet implemented")
    }

    override fun onUpgrade(p0: SQLiteDatabase?, p1: Int, p2: Int) {
        TODO("Not yet implemented")
    }
}
```

assets에서 복사하지 않고 그냥 기존에 존재하던 SQLite를 Room으로 이전할 때는 `DatabaseCopier.kt`의 `private fun copyAttachedDatabase(context: Context)`와 `private fun copyDatabaseFromAssets(context: Context, path: File)` 함수가 필요 없다.

https://bearhunter49.tistory.com/3
https://developer.android.com/codelabs/android-room-with-a-view-kotlin?hl=ko
https://parkho79.tistory.com/132
https://hrankit.github.io/RoomSQLiteDifferenceFinder/
https://stackoverflow.com/questions/44411928/how-to-annotate-column-as-not-null-using-android-room-persistence-library
https://www.geeksforgeeks.org/how-to-pre-populate-database-in-android-using-sqlite-database/
