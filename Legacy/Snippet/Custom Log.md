---
boostnote:
  createdAt: '2022-02-21T02:09:23.093Z'
  updatedAt: '2022-03-07T02:43:34.923Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Custom Log
  tags:
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: ddc2b8e5-2a91-4159-a29a-2e8df0f85bfd
  storage: d112f8ec1e85e056a09d
---

Custom Log
---
```kotlin
object Logger {
    private const val TAG = "LottoLog"

    // 에러
    fun e(msg: String?) {
        if (BuildConfig.DEBUG) {
            Log.e(TAG, prepareLog(msg))
        }
    }
    
    // 경고
    fun w(msg: String?) {
        if (BuildConfig.DEBUG) {
            Log.w(TAG, prepareLog(msg))
        }
    }

    // 디버깅 출력 (필터링 가능)
    fun d(msg: String?) {
        if (BuildConfig.DEBUG) {
            Log.d(TAG, prepareLog(msg))
        }
    }

    // 정보성 메시지
    fun i(msg: String?) {
        if (BuildConfig.DEBUG) {
            Log.i(TAG, prepareLog(msg))
        }
    }

    // 개발 전용
    fun v(msg: String?) {
        if (BuildConfig.DEBUG) {
            Log.v(TAG, prepareLog(msg))
        }
    }

    private fun prepareLog(msg: String?): String {
        val ste = Thread.currentThread().stackTrace[4]
        val className = if (ste.fileName.endsWith(".kt")) {
            ste.fileName.replace(".kt", "")
        } else {
            ste.fileName.replace(".java", "")
        }

        return StringBuilder().append("[").append(className).append("::").append(ste.methodName).append("] ").append(msg).toString()
    }
}
```