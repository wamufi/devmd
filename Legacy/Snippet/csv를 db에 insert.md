csv를 db에 insert
---
경로는
서버 -> zip 다운로드 -> zip 저장 -> 압축 파일 선정 -> unzip -> csv를 db에 insert
updateHashTag() -> requestHashTagMetaInfo() -> hashTagMetaInfoUseCase -> runUpdateData() -> needUpdateData() 
-> requestHashTagFile() -> downloadUseCase -> writeResponseBodyToDisk() -> unzipDownloadFile() -> updateHashTagTable() -> insertCSVToTable() -> deleteHashTagTable() -> deleteAllHashTagKeywordListUseCase -> insertHashTagKeywordListUseCase


```kotlin
class HashTagManager @Inject constructor(
    private val prefs: AppSharedPreferences,
    private val hashTagMetaInfoUseCase: HashTagMetaInfoUseCase,
    private val downloadUseCase: DownloadUseCase,
    private val insertHashTagKeywordListUseCase: InsertHashTagKeywordListUseCase,
    private val deleteAllHashTagKeywordListUseCase: DeleteAllHashTagKeywordListUseCase
) {
    private var info: HashTagMetaInfoResponse? = null

    //region HashTagManager.java
    /***
     * 해시태그 정보 업데이트
     */
    fun updateHashTag() {
        CoroutineScope(Dispatchers.IO).launch {
            // 서버로 부터 시퀀스 정보를 조회하여 현재 DB에 저장되어있는 버젼보다 높은 경우 파일 취득
            requestHashTagMetaInfo()
        }
    }

    /***
     * 해시태그정보 업데이트가 필요한 경우 업데이트 실행
     * @param info
     */
    private fun runUpdateData(info: HashTagMetaInfoResponse) {
        this.info = info
        // 업데이트가 필요한 경우
        if (needUpdateData(info.version)) {
            // 파일 다운로드 및 저장
            requestHashTagFile(info.targetFileUrl)
        }
    }

    /***
     * 해시테그 메타정보의 버젼정보를 현재버전과 비교하여 체크
     * @param currentVersion
     * @return
     */
    private fun needUpdateData(version: String): Boolean {
        val currentVersion = if (version.isEmpty()) {
            0
        } else {
            version.toInt()
        }

        // 현재 저장되어 있는 버전과 서버의 버전을 비교하여 업데이트 여부 결정
        val prevVersion = prefs[PrefKey.SPU_HASHTAG_SEQ_NUMBER] ?: 0

        // 현재 버전이 로컬 버전보다 높을 경우 업데이트 진행
        return currentVersion > prevVersion
    }


    /***
     * 해시태그 정보 테이블에 저장
     * @param csvName
     */
    private fun updateHashTagTable(csvName: String): Boolean {
        val file = File("${WhoWhoApp.instance.filesDir}${File.separator}$TAG_FOLDER${File.separator}$csvName")
        return insertCSVToTable(file)
    }
    //endregion

    //region TagAPIManager.java
    /***
     * 해시태그 메타정보 취득
     */
    private suspend fun requestHashTagMetaInfo() {
        val userId = prefs[PrefKey.SPU_K_USER_ID] ?: ""
        val userPh = WhoWhoApp.instance.getPhoneNumber()
        val data = HashTagMetaInfoDTO(addCommonParam(prefs), userId, userPh, "HASHTAG")

        hashTagMetaInfoUseCase(data).collect { result ->
            when (result) {
                is DataResult.Success -> {
                    runUpdateData(result.data) // TagAPIManager.OnMetaDataListener()
                }
                is DataResult.Error -> {

                }
                is DataResult.Empty -> {

                }
            }
        }
    }

    /***
     * 해시태그 파일 취득
     * @param url
     * @param listener
     */
    private fun requestHashTagFile(url: String) {
        CoroutineScope(Dispatchers.IO).launch {
            var csvName = ""
            downloadUseCase(url).apply {
                if (writeResponseBodyToDisk(this)) {
                    // 파일 압축 풀기
                    csvName = unzipDownloadFile()
                }
            }

            withContext(Dispatchers.Main) { // TagAPIManager.OnDataSaveListener()
                if (csvName.isNotEmpty() && updateHashTagTable(csvName)) {
                    prefs[PrefKey.SPU_HASHTAG_SEQ_NUMBER] = info?.version?.toInt()
                }
            }
        }
    }
    //endregion

    //region TagFileManager.java
    var TAG_FOLDER = "hashtag"
    var TAG_ZIP_FILENAME = "tagDownloadFile.zip"

    /***
     * 수신한파일 내용 저장
     * @param body
     * @return
     */
    private fun writeResponseBodyToDisk(body: ResponseBody): Boolean {
        var isRet = true

        val path = "${WhoWhoApp.instance.filesDir}${File.separator}$TAG_FOLDER"
        val downloadFile = File("$path${File.separator}$TAG_ZIP_FILENAME")
        val filePath = File(path)
        if (!filePath.exists()) { filePath.mkdir() }

        try {
            body.byteStream().use { input ->
                FileOutputStream(downloadFile).use { output ->
                    val buffer = ByteArray(4 * 1024)
                    var read: Int
                    while (input.read(buffer).also { read = it } != -1) {
                        output.write(buffer, 0, read)
                    }
                    output.flush()
                }

            }
        } catch (e: Exception) {
            e.printStackTrace()
            isRet = false
        }

        return isRet
    }


    /***
     * 압축 해제 대상 파일 선정 풀기
     * @return
     */
    private fun unzipDownloadFile(): String {
        var unzipFileName = ""

        val path = "${WhoWhoApp.instance.filesDir}${File.separator}$TAG_FOLDER"
        val file = File("$path${File.separator}$TAG_ZIP_FILENAME")
        if (file.exists()) {
            val targetDir = File(WhoWhoApp.instance.filesDir, TAG_FOLDER)
            if (!targetDir.exists()) { targetDir.mkdir() }

                unzipFileName = extractFolder(path, file)
            if (unzipFileName.isEmpty()) {
                file.delete()
            }
        } else {
            Log.e("_lmh", "unzip temp file not exist")
        }

        return unzipFileName
    }

    /***
     * 압축 풀기
     * @param destPath
     * @param file
     * @return
     * @throws IOException
     */
    private fun extractFolder(destPath: String, file: File): String {
        var fileName = ""

        val buffer = ByteArray(8 * 1024)
        val zipFile = ZipFile(file)
        val dir = File(destPath)
        if (!dir.exists()) { dir.mkdir() }

        try {
            zipFile.entries().asSequence().forEach { zipEntry ->
                var currentEntry = zipEntry.name
                val n = currentEntry.indexOf("/")
                if (n > -1) {
                    currentEntry = currentEntry.substring(n + 1, currentEntry.length)
                }

                fileName = currentEntry

                val destFile = File(destPath, currentEntry)
                if (!destFile.absolutePath.startsWith(destPath)) {
                    throw SecurityException("path is wrong!")
                }
                destFile.parentFile?.mkdirs()

                if (!zipEntry.isDirectory) {
                    BufferedInputStream(zipFile.getInputStream(zipEntry)).use { input ->
                        destFile.outputStream().buffered(buffer.size).use { output ->
                            var read: Int
                            while (input.read(buffer).also { read = it } != -1) {
                                output.write(buffer, 0, read)
                            }
                            output.flush()
                        }
                    }
                }
            }
        } catch (e: Exception) {
            Log.d("ExtractFolder", "Exception: $e")
        }

        return fileName
    }
    //endregion

    //region TagDBManager.java
    // 테이블 데이터 넣기
    private fun insertCSVToTable(file: File): Boolean {
        deleteHashTagTable()

        val reader = BufferedReader(FileReader(file))
        try {
            reader.lineSequence().filter {
                it.isNotBlank()
            }.map {
                val string = it.split(',', ignoreCase = false)
                val hashTagKeywordList = HashTagKeywordList(
                    string[0],
                    string[1],
                    string[2],
                    string[3],
                    string[4],
                    string[5],
                    string[6],
                    string[7],
                    string[8],
                    string[9],
                    string[10]
                )

                CoroutineScope(Dispatchers.IO).launch {
                    insertHashTagKeywordListUseCase(hashTagKeywordList)
                }
            }.toList()
        } catch (e: Exception) {
            e.printStackTrace()
            return false
        }

        return true
    }

    // 테이블 삭제
    private fun deleteHashTagTable() {
        CoroutineScope(Dispatchers.IO).launch {
            deleteAllHashTagKeywordListUseCase()
        }
    }
    //endregion
}
```

https://louet.tistory.com/187
https://velog.io/@windsekirun/Kotlin-Unzip-Zip-files-in-Android
https://mobikul.com/room-database-export-import-to-csv/
