---
boostnote:
  createdAt: '2022-03-22T06:02:03.519Z'
  updatedAt: '2022-03-22T06:10:39.484Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Lottie로 로딩 화면 만들기
  tags:
    - Android
    - Kotlin
    - Lottie
    - Dialog
  linesHighlighted: []
  isStarred: false
  key: 64c0bf12-36be-4e31-bb04-99a4d7f0044a
  storage: d112f8ec1e85e056a09d
---

Lottie로 로딩 화면 만들기
---
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.airbnb.lottie.LottieAnimationView
        android:layout_width="200dp"
        android:layout_height="150dp"
        android:layout_gravity="center"
        android:tag="0907"
        app:lottie_autoPlay="true"
        app:lottie_loop="true"
        app:lottie_rawRes="@raw/loading" />

</FrameLayout>
```

<h4>DialogFragment로 처리</h4>
LoadingDialogFragment.kt
```kotlin
class LoadingDialogFragment : DialogFragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        dialog?.requestWindowFeature(Window.FEATURE_NO_TITLE)
        dialog?.window?.setBackgroundDrawable(ColorDrawable(Color.TRANSPARENT))

        isCancelable = false

        return inflater.inflate(R.layout.dialog_loading, container, false)
    }

    override fun show(manager: FragmentManager, tag: String?) {
        try {
            val fragmentTransaction = manager.beginTransaction()
            fragmentTransaction.add(this, tag)
            fragmentTransaction.commitAllowingStateLoss()
        } catch (ignored: IllegalStateException) {

        }
    }
}
```

BaseActivity.kt
```kotlin
// 로딩 애니메이션 보여주기
fun AppCompatActivity.showAnimationIndicator() {
    val fragment = supportFragmentManager.findFragmentByTag("0907")
    if (fragment != null) {
        return
    }

    val dialog = LoadingDialogFragment()
    dialog.show(supportFragmentManager, "0907")
}

// 로딩 애니메이션 숨기기
fun AppCompatActivity.hideAnimationIndicator() {
    val fragment = supportFragmentManager.findFragmentByTag("0907")
    if (fragment != null) {
        val dialogFragment = fragment as DialogFragment
        dialogFragment.dismiss()
    }
}
```

Fragment.kt
```kotlin
private fun getHistoryList() {
    val parent = activity as BaseActivity
    parent.showAnimationIndicator()

    hmDBViewModel.getNumHistoryQuery("0", "30")
    hmDBViewModel.lottoInfoList.observe(viewLifecycleOwner) {
        lottoInfoList = it
        adapter.submitList(it)
        parent.hideAnimationIndicator()
    }
}
```

<h4>rootView로 처리</h4>

BaseActivity.kt
```kotlin
public override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    initLoadingView()
}

// 로딩
var loadingView: View? = null

fun showProgressBar(show: Boolean) {
    loadingView?.visibility = if (show) View.VISIBLE else View.GONE
}

private fun initLoadingView() {
    val rootView = window.decorView.rootView
    if (rootView is FrameLayout) {
        loadingView = layoutInflater.inflate(R.layout.layout_loading, null)
        loadingView?.visibility = View.GONE
        rootView.addView(loadingView)
    }
}
```

ViewModel.kt
```kotlin
private val _dataLoading = MutableLiveData<Boolean>()
val dataLoading: LiveData<Boolean> = _dataLoading

fun getButtonList() {
    _dataLoading.value = true
    viewModelScope.launch {
        repository.getButtonList().apply {
            _dataLoading.value = false
            _buttonList.value?.clear()
            buttonList.value?.clear()

            if (isSuccessful) {
                body()?.let {
                    _buttonList.value = it.data?.buttonList as MutableList<ButtonList.Data.Button>
                }
            } else {
                Log.d("getButtonList", "error ${errorBody()}")
            }
        }
    }
}
```

Fragment.kt
```kotlin
buttonViewModel.dataLoading.observe(this, Observer { bool ->
    showProgressBar(bool)
})
```