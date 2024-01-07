---
boostnote:
  createdAt: '2022-02-14T07:59:08.140Z'
  updatedAt: '2022-03-22T06:12:59.621Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: DialogFragment
  tags:
    - Android
    - Kotlin
    - Dialog
  linesHighlighted: []
  isStarred: false
  key: 39c71f74-a375-41ab-800d-8ea5ed585a76
  storage: d112f8ec1e85e056a09d
---

DialogFragment
---
UIAlertController 같은 거 만들기
```kotlin
class ScanDialogFragment : DialogFragment() {
    override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        return activity?.let {
            val builder = AlertDialog.Builder(it)
            builder.setMessage("QR 스캔을 위한 카메라 권한 설정이 필요합니다.").setPositiveButton(R.string.ok) { dialog, _ ->
                dialog.dismiss()
                it.finish()
            }
            builder.create()
        } ?: throw IllegalStateException("Activity cannot be null")
    }
}
```

```kotlin
val alertDialog = MaterialAlertDialogBuilder(this)
alertDialog.setMessage("네트워크 연결이 필요합니다.").setPositiveButton(android.R.string.ok) { dialog, _ ->
    dialog.dismiss()
    finish()
}
alertDialog.create()
alertDialog.show()
```