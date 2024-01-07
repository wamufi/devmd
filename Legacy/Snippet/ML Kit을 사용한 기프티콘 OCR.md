---
title: "ML Kit을 사용한 기프티콘 OCR"
date: 2023-05-09
time: 13:14:00
tags:
  - android
  - mlkit
  - kotlin
id: 20230509131400
---
build.gradle (:app)
``` 
dependencies {
    // ML Kit
    implementation 'com.google.mlkit:text-recognition-korean:16.0.0' // 텍스트 인식
    implementation 'com.google.mlkit:barcode-scanning:17.1.0' // 바코드 스캔
}
```
추가

MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    private var imageUri: Uri? = null
    private val pickMedia = registerForActivityResult(ActivityResultContracts.PickVisualMedia()) {
        it?.let {
            Log.d("promptPhoto", "uri: $it")
            imageUri = it

            binding.indicator.show()
            detectBarcode()
            recognizedText()
        } ?: run {
            Log.d("promptPhoto", "uri is null")
        }
    }

    private var barcodeBoundingBox: Rect? = null // 바코드 좌표

    override fun onCreate(savedInstanceState: Bundle?) {
        WindowCompat.setDecorFitsSystemWindows(window, false)
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.ocrTextview.movementMethod = ScrollingMovementMethod()

        binding.fab.setOnClickListener {
            promptPhoto()
        }
    }

    override fun onResume() {
        super.onResume()

        if (imageUri == null) {
            promptPhoto()
        }
    }

    override fun onCreateOptionsMenu(menu: Menu): Boolean {
        // Inflate the menu; this adds items to the action bar if it is present.
        menuInflater.inflate(R.menu.menu_main, menu)
        return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        return when (item.itemId) {
            R.id.action_settings -> true
            else -> super.onOptionsItemSelected(item)
        }
    }

    private fun promptPhoto() {
        pickMedia.launch(PickVisualMediaRequest(ActivityResultContracts.PickVisualMedia.ImageOnly))
    }

    private fun getImageViewLocationOnScreen(): Rect {
        val location = IntArray(2)
        binding.ocrImageview.getLocationOnScreen(location)

        val x = location[0]
        val y = location[1]

        return Rect(x, y, binding.ocrImageview.width, binding.ocrImageview.height)
    }

    // 바코드 탐지
    private fun detectBarcode() {
        if (imageUri == null) return

        val image: InputImage = InputImage.fromFilePath(applicationContext, imageUri!!)
//        try {
//            image = InputImage.fromFilePath(applicationContext, imageUri!!)
//        } catch (e: Exception) {
//            e.printStackTrace()
//        }

        val scanner = BarcodeScanning.getClient(BarcodeScannerOptions.Builder().setBarcodeFormats(Barcode.FORMAT_CODE_128).build())
        scanner.process(image).addOnSuccessListener {
            Log.d("detectBarcode", "barcodes: $it")
            handleDetectedBarcodes(it)
        }
    }

    // 텍스트 인식
    private fun recognizedText() {
        if (imageUri == null) return

        binding.ocrImageview.setImageURI(imageUri)

        val image: InputImage = InputImage.fromFilePath(applicationContext, imageUri!!)
//        try {
//            image = InputImage.fromFilePath(applicationContext, imageUri!!)
//        } catch (e: Exception) {
//            e.printStackTrace()
//        }

        val recognizer = TextRecognition.getClient(KoreanTextRecognizerOptions.Builder().build())
        recognizer.process(image).addOnSuccessListener {
            Log.d("recognizedText", "visionText: ${it.textBlocks}")
            handleRecognizedText(it)
        }
    }

    // 바코드 탐지 성공
    private fun handleDetectedBarcodes(barcodes: List<Barcode>) {
        barcodes.forEach { barcode ->
            barcodeBoundingBox = barcode.boundingBox
            Log.d("handleDetectedBarcodes", "${barcode.boundingBox}")
        }
    }

    // 텍스트 인식 성공
    private fun handleRecognizedText(text: Text) {
        binding.indicator.hide()

        val texts: MutableList<String> = mutableListOf()

        var brandName = "" // 교환처
        var productName = "" // 상품명
        var expireDate = "" // 유효기간

        var brandCenterY = 0.0f
        var expireDateCenterY = 0.0f

        val dateStrings = listOf("기간", "기한")
        val brandStrings = listOf("교환처", "사용처")

        text.textBlocks.forEach { block ->
            val blockText = block.text
            texts.add(blockText)

            // '기간, 기한' 등의 단어가 있을 경우 그 단어의 centerY 값 저장
            if (dateStrings.any { it in blockText }) {
                expireDateCenterY = block.boundingBox?.exactCenterY() ?: 0.0f
                Log.d("handleRecognizedText", "expireDateCenterY: $expireDateCenterY, text: $blockText")
            }

            // '교환처, 사용처' 등의 단어가 있을 경우 그 단어의 midY 값 저장
            if (brandStrings.any { it in blockText}) {
                brandCenterY = block.boundingBox?.exactCenterY() ?: 0.0f
                Log.d("handleRecognizedText", "brandCenterY: $brandCenterY, text: $blockText")
            }

            // 저장한 '기간'의 midY 값이 있을 경우 좌표값(midY)을 비교하여 날짜를 가져오기
            if (expireDateCenterY > 0) {
                val centerY = block.boundingBox?.exactCenterY() ?: 0.0f
                if (abs(centerY - expireDateCenterY) <= 5) { // 좌표값 비교
                    Log.d("handleRecognizedText", "expireDateCenterY > 0 -- text: ${blockText.getDate()}")
                    expireDate = blockText.getDate() ?: ""
                    return@forEach
                }
            }

            // 저장한 '교환처'의 midY 값이 있을 경우 좌표값(midY)을 비교하여 교환처 가져오기
            if (brandCenterY > 0) {
                val centerY = block.boundingBox?.exactCenterY() ?: 0.0f
                if (abs(centerY - brandCenterY) <= 5) { // 좌표값 비교
                    Log.d("handleRecognizedText", "brandCenterY > 0 -- text: $blockText")
                    brandName = blockText
                    return@forEach
                }
            }

            Log.d("handleRecognizedText", "text: $blockText, lines: ${block.lines}, boundingBox: ${block.boundingBox}, centerY: ${block.boundingBox?.exactCenterY()} cornerPoints: ${block.cornerPoints}")
        }

        // '기간' 단어가 없을 경우 임의의 날짜 형식 텍스트 가져오기
        expireDate.ifEmpty {
            val dates = texts.filter { !it.getDate().isNullOrEmpty() }
            if (dates.isNotEmpty()) {
                expireDate = dates[0]
                Log.d("handleRecognizedText", "expireDate.isEmpty -- expireDate: $expireDate")
            }
        }

        val imageViewLocation = getImageViewLocationOnScreen()
        var productTextBlock: TextBlock? = null

        // 상품명 가져오기
        barcodeBoundingBox?.let { barcode -> // 바코드 인식했을 경우
            Log.d("handleRecognizedText", "imageViewLocation: $imageViewLocation")

            productTextBlock = text.textBlocks.filter { block ->
                block.boundingBox?.let {
                    block.text.hasContainKorean() && it.bottom <= barcode.top && it.left < imageViewLocation.width() * 0.5 && it.width() > imageViewLocation.width() * 0.28 // 한국어 포함 여부 && 바코드 위의 값 && x값이 화면의 절반을 넘지 않는 값 && 너비 0.28 이상
                            && !(dateStrings.any { string -> string in block.text }) // && '기간' 텍스트 미포함
                            && block.lines.any { line -> line.confidence > 0.5 } // && 각 라인 신뢰도 0.5 이상
                } ?: false
            }.maxByOrNull {
                it.boundingBox!!.height() // 높이가 가장 큰 값
            }

            if ((productTextBlock?.lines?.count() ?: 0) > 1) {
                productTextBlock?.lines?.forEach { // 2줄 이상
                    productName += it.text
                }
            } else {
                productName = productTextBlock?.text ?: ""
            }
        } ?: run { // 바코드 인식 못했을 경우
            productTextBlock = text.textBlocks.filter { block ->
                block.boundingBox?.let {
                    block.text.hasContainKorean() && it.left < imageViewLocation.width() * 0.5 && it.width() > imageViewLocation.width() * 0.28 // 한국어 포함 여부 && x값이 화면의 절반을 넘지 않는 값 && 너비 0.28 이상
                            && !(dateStrings.any { string -> string in block.text }) // && '기간' 텍스트 미포함
                            && block.lines.any { line -> line.confidence > 0.5 } // && 각 라인 신뢰도 0.5 이상
                } ?: false
            }.maxByOrNull {
                it.boundingBox!!.height() // 높이가 가장 큰 값
            }

            if ((productTextBlock?.lines?.count() ?: 0) > 1) {
                productTextBlock?.lines?.forEach { // 2줄 이상
                    productName += it.text
                }
            } else {
                productName = productTextBlock?.text ?: ""
            }
        }

        // '교환처' 단어가 없지만 상품명이 있을 경우 상품명 바로 이전 값 가져오기
        if (brandName.isEmpty() && productTextBlock != null) {
            val result = text.textBlocks.minBy {
                abs((productTextBlock!!.boundingBox?.top ?: 0) - (it.boundingBox?.bottom ?: 0))
            }
            brandName = result.text
            Log.d("handleRecognizedText", "brandName.isEmpty && productObservation != nil -- brandName: $brandName")
        }

        binding.ocrTextview.text = "교환처: $brandName\n상품명: $productName\n유효기간: $expireDate"
    }
}
```

Extensions.kt
```kotlin
// 날짜 가져오기
// ex. 2023년 1월 1일, 2023.01.01, 2023.1.1
fun String.getDate(): String? {
    val regex = "\\d{2,4}년?[-|.|\\s|\\/]\\d{1,2}월?[-|.|\\s|\\/]\\d{1,2}일?".toRegex()
    val matchResult = regex.find(this)

    return matchResult?.value
}

// 한국어 포함 여부
fun String.hasContainKorean(): Boolean {
    val regex = "[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]".toRegex()
    val matchResult = regex.find(this)

    return matchResult != null
}
```

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/ocr_imageview"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:adjustViewBounds="true"
        app:layout_constraintBottom_toTopOf="@id/ocr_textview"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.8"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/ocr_textview"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:paddingHorizontal="8dp"
        android:scrollbars="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.2"
        app:layout_constraintStart_toStartOf="parent" />


    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="@dimen/fab_margin"
        android:layout_marginBottom="16dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:srcCompat="@android:drawable/ic_menu_camera" />

    <com.google.android.material.progressindicator.CircularProgressIndicator
        android:id="@+id/indicator"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:indeterminate="true"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```