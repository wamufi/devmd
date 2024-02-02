---
title: Log class 만들기
date: 2024-01-31
time: 13:52:37
tags:
  - android
  - kotlin
  - ios
  - swift
---
```kotlin
class DbLog {
    companion object {
        private fun getLogInfo(): String {
            val fullClassName = Thread.currentThread().stackTrace[4].className
            val className = fullClassName.substring(fullClassName.lastIndexOf(".") + 1)
            val methodName = Thread.currentThread().stackTrace[4].methodName
            val lineNumber = Thread.currentThread().stackTrace[4].lineNumber

            return "$className.$methodName()[$lineNumber]"
        }

        fun d(msg: String) {
            if (BuildConfig.DEBUG) {
                Log.d("DBLog", getLogInfo() + " | " + msg)
            }
        }
    }
}
```

```kotlin
object LogHelper {
    private const val TAG = "AppName"

    fun d(message: Any) {
        if (BuildConfig.DEBUG) {
            Log.d(TAG, "🔧 Debug ${convertMessage(convertToString(message))}")
        }
    }

	fun i(message: Any) {
		if (BuildConfig.DEBUG) {
			Log.i(TAG, "ℹ️ Info ${convertMessage(convertToString(message))}")
		}
	}

    fun e(message: Any) {
        if (BuildConfig.DEBUG) {
            Log.e(TAG, "❌ Error ${convertMessage(convertToString(message))}")
        }
    }

    private fun convertMessage(message: String): String {
        val element = Thread.currentThread().stackTrace[4]
        val fileName = element.fileName
        return String.format(
            Locale.getDefault(),
            "[%s.%s()#%d] %s",
            fileName.substring(0, fileName.indexOf(".")),
            element.methodName,
            element.lineNumber,
            message
        )
    }

    private fun convertToString(obj: Any): String {
        return if (obj is String) obj.toString() else {
            try {
                obj.toString()
            } catch (e: Exception) {
                ""
            }
        }
    }
}
```

```swift
import Foundation

fileprivate enum LogType {
    case debug, info, error
}

fileprivate var dateFormatter: DateFormatter = {
    let formatter = DateFormatter()
    formatter.dateFormat = "yyyy-MM-dd HH:mm:ss.SSS "
    return formatter
}()

fileprivate func convertMessage(_ items: [Any], type: LogType, _ filePath: String, _ function: String, _ line: Int) -> String {
    let time = dateFormatter.string(from: Date())
    
    let lastSlashIndex = (filePath.lastIndex(of: "/") ?? String.Index(utf16Offset: 0, in: filePath))
    let nextIndex = filePath.index(after: lastSlashIndex)
    let fileName = filePath.suffix(from: nextIndex).replacingOccurrences(of: ".swift", with: "")
    
    let message = items.map {"\($0)"}.joined(separator: " ")
    
    switch type {
    case .debug:
        return "\(time) 🔧 Debug [\(fileName).\(function)#\(line)] \(message)"
    case .info:
        return "\(time) ℹ️ Info [\(fileName).\(function)#\(line)] \(message)"
    case .error:
        return "\(time) ❌ Error [\(fileName).\(function)#\(line)] \(message)"
    }
}

func logDebug(items: Any..., tag: String = "", filePath: String = #file, function: String = #function, line: Int = #line) {
    #if DEBUG
    print(convertMessage(items, type: .debug, filePath, function, line))
    #endif
}

func logInfo(items: Any..., tag: String = "", filePath: String = #file, function: String = #function, line: Int = #line) {
    #if DEBUG
    print(convertMessage(items, type: .info, filePath, function, line))
    #endif
}

func logError(items: Any..., tag: String = "", filePath: String = #file, function: String = #function, line: Int = #line) {
    #if DEBUG
    print(convertMessage(items, type: .error, filePath, function, line))
    #endif
}
```