---
title: DownloadManager
date:  2025-04-14
time:  09:52:04
tags:
  - android
  - kotlin
---
DownloadManager.kt
```kotlin
class DownloadManager @Inject constructor(private val context: Context) {  
  
    /**  
     * 내부 저장소에 파일 저장  
     */  
    fun saveFileToInternalStorage(body: ResponseBody, fileName: String): File {  
        val file = File(context.filesDir, fileName)  
  
        /* 버퍼로 처리 (progress % 가능)  
        val inputStream: InputStream = body.byteStream()        val outputStream = FileOutputStream(file)        val buffer = ByteArray(1024)        var length: Int        while (inputStream.read(buffer).also { length = it } != -1) {            outputStream.write(buffer, 0, length)//            downloadedBytes += length  
        }        outputStream.close()        inputStream.close()  
        return file         */  
        val inputStream = body.byteStream()  
        val outputStream = FileOutputStream(file)  
        inputStream.copyTo(outputStream)  
        outputStream.close()  
        inputStream.close()  
  
        LogHelper.v("<-- SAVE $file -->")  
  
        return file  
    }  
  
    /**  
     * 내부 저장소 파일들 가져오기  
     */  
    fun loadFilesFromInternalStorage() = run { context.filesDir.listFiles()?.filter { it.isFile } ?: emptyList() }  
  
    /**  
     * 내부 저장소 파일 가져오기  
     */  
    fun loadFileFromInternalStorage(fileName: String): File? {  
        val file = File(context.filesDir, fileName)  
        LogHelper.v("<-- LOAD $file -->")  
        return if (file.isFile) file else null  
    }  
  
    /**  
     * 내부 저장소 파일 삭제  
     */  
    fun deleteFileFromInternalStorage(fileName: String) {  
        val file = File(context.filesDir, fileName)  
        file.deleteRecursively()  
        LogHelper.v("<-- DELETE $file -->")  
    }  
    /**  
     * 내부 저장소 파일 찾기  
     */  
    fun findLocalFile(fileName: String? = null, regex: Regex? = null): File? {  
        return if (regex != null) {  
            loadFilesFromInternalStorage().find { it.name.matches(regex) } // 전체 문자열이 정규식과 정확히 일치하는지 확인 (ex. regex: "text", file: "my_test_file" -> false)//            loadFilesFromInternalStorage().find { regex.find(it.name) != null } // 문자열에 특정 패턴이 포함되었는지 확인 (ex. regex: "text", file: "my_test_file" -> true)        } else if (fileName != null) {  
            loadFileFromInternalStorage(fileName)  
        } else {  
            null  
        }  
    }  
    /**  
     * 외부 저장소 파일 가져오기  
     */  
    fun loadFilesFromExternalStorage(context: Context): List<File> {  
        val downloadDir = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS)  
        LogHelper.d(downloadDir)  
        return downloadDir?.listFiles()?.filter { it.isFile } ?: emptyList()  
    }
}
```

AppModule.kt
```kotlin
@Module
@InstallIn(SngletonComponent::class)
object AppModule {
	@Provides  
	@Singleton  
	fun provideDownloadManager(@ApplicationContext context: Context): DownloadManager {  
	    return DownloadManager(context)  
	}
}
```