---
boostnote:
  createdAt: '2022-03-23T05:22:34.371Z'
  updatedAt: '2022-03-23T09:30:22.386Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: RecyclerView에 값이 없을 때
  tags:
    - Android
    - Kotlin
    - RecyclerView
  linesHighlighted: []
  isStarred: false
  key: f5116689-06cc-4795-9937-322ecb105ebd
  storage: d112f8ec1e85e056a09d
---

RecyclerView에 값이 없을 때
---
RecyclerViewEmptySupport.kt
```kotlin
class RecyclerViewEmptySupport : RecyclerView {
    var emptyView: View? = null
    private val emptyObserver: AdapterDataObserver = object : AdapterDataObserver() {
        override fun onChanged() {
            val adapter = adapter
            if (adapter != null && emptyView != null) {
                if (adapter.itemCount == 0) {
                    emptyView!!.visibility = View.VISIBLE
                    this@RecyclerViewEmptySupport.visibility = View.GONE
                } else {
                    emptyView!!.visibility = View.GONE
                    this@RecyclerViewEmptySupport.visibility = View.VISIBLE
                }
            }
        }
    }

    override fun setAdapter(adapter: Adapter<*>?) {
        super.setAdapter(adapter)
        adapter?.registerAdapterDataObserver(emptyObserver)
        emptyObserver.onChanged()
    }

    constructor(context: Context) : super(context)
    constructor(context: Context, @Nullable attrs: AttributeSet?) : super(context, attrs)
    constructor(context: Context, @Nullable attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr)
}
```
fragment.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.hmgameplay.lotto.ui.RecyclerViewEmptySupport
        android:id="@+id/addmynum_history_recycler"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:nestedScrollingEnabled="false" />

    <TextView
        android:id="@+id/addmynum_history_empty"
        style="@style/TextAppearance.Material3.TitleMedium"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="로또 번호를 6개 선택해주세요." />

</RelativeLayout>
```

Fragment.kt
```kotlin
val recyclerView = binding.addmynumHistoryRecycler
recyclerView.layoutManager = LinearLayoutManager(context)
recyclerView.adapter = AddMyNumHistoryAdapter(pastInfoList, selectedNumbers)
recyclerView.emptyView = binding.addmynumHistoryEmpty
```

[[Android] 리사이클러뷰의 리스트에 데이터가 없을 때 '데이터가 없습니다' 텍스트뷰 띄우는 방법](https://onlyfor-me-blog.tistory.com/299)
