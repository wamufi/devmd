Utils
---
```kotlin
// 갤럭시 게임 런처 실행 중인가
fun isGameInterruption(contentResolver: ContentResolver): Boolean {
    return Settings.System.getInt(contentResolver, "game_no_interruption", 0) == 1
}

// 서비스가 실행 중인가
// getRunningServices is deprecated
@Suppress("DEPRECATION")
fun <T> Context.isServiceRunning(service: Class<T>): Boolean {
    return (getSystemService(ACTIVITY_SERVICE) as ActivityManager).getRunningServices(Integer.MAX_VALUE)
        .any { it -> it.service.className == service.name }
}

// Swift: guard let
inline fun <T : Any> guardLet(vararg elements: T?, closure: () -> Nothing): List<T> {
    return if (elements.all { it != null }) {
        elements.filterNotNull()
    } else {
        closure()
    }
}

// Swift: if let
inline fun <T : Any> ifLet(vararg elements: T?, closure: (List<T>) -> Unit) {
    if (elements.all { it != null }) {
        closure(elements.filterNotNull())
    }
}

// 파라미터 두 개가 null이 아닐 때
inline fun <T1 : Any, T2 : Any, R : Any> safeLet(p1: T1?, p2: T2?, block: (T1, T2) -> R?): R? {
    return if (p1 != null && p2 != null) block(p1, p2) else null
}

// 파라미터 전부 null이 아닐 때
inline fun <T : Any, R : Any> whenAllNotNull(vararg options: T?, block: (List<T>) -> R) {
    if (options.all { it != null }) {
        block(options.filterNotNull()) // or do unsafe cast to non null collection
    }
}

inline fun <T, R> T?.isNotNull(func: (T) -> R) = Ternary(
    if (this != null) {
        func(this)
    } else null
)

inline fun String?.isNotEmpty(func: (String) -> Unit) = Ternary(
    if (this.isNullOrBlank() || "null" == this) {
        null
    } else func(this)

)

inline fun <R> Ternary<R>.isEmpty(func: () -> R) = value ?: func()
inline fun <R> Ternary<R>.isNull(func: () -> R) = value ?: func()
```

```java
// 앱 사이즈 확인
PackageManager packageManager = getPackageManager();
ApplicationInfo applicationInfo = null;
try {
    applicationInfo = packageManager.getApplicationInfo(getPackageName(), 0);
} catch (PackageManager.NameNotFoundException e) {
    throw new RuntimeException(e);
}
File file = new File(applicationInfo.publicSourceDir);
long size = file.length();
Log.d("SIZE: "+size);

if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O) {
    StorageStatsManager storageStatsManager = (StorageStatsManager) getApplicationContext().getSystemService(Context.STORAGE_STATS_SERVICE);
    try {
        ApplicationInfo ai = getApplicationContext().getPackageManager().getApplicationInfo(getPackageName(), 0);
        StorageStats storageStats = storageStatsManager.queryStatsForUid(ai.storageUuid, getApplicationInfo().uid);
        long cacheSize = storageStats.getCacheBytes();
        long dataSize = storageStats.getDataBytes();
        long apkSize = storageStats.getAppBytes();
        Log.d("cacheSize: " + cacheSize + " dataSize: " + dataSize + " apkSize: " + apkSize);
    } catch (Exception e) {
    }
}
```