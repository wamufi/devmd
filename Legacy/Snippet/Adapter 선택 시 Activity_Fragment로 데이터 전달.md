---
boostnote:
  createdAt: '2022-03-02T07:01:21.647Z'
  updatedAt: '2022-03-07T02:42:48.413Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Adapter 선택 시 Activity/Fragment로 데이터 전달
  tags:
    - Android
    - Kotlin
    - Adapter
    - RecyclerView
  linesHighlighted: []
  isStarred: false
  key: 73cf0838-da6b-4a2e-aa5f-30b91420f1d2
  storage: d112f8ec1e85e056a09d
---

Adapter 선택 시 Activity/Fragment로 데이터 전달
---
```kotlin
class AddMyNumAdapter(private val onClick: (String) -> Unit) : RecyclerView.Adapter<AddMyNumAdapter.ViewHolder>() {

    private lateinit var context: Context

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        context = parent.context

        val binding = AdapterAddmynumBinding.inflate(LayoutInflater.from(context), parent, false)
        return ViewHolder(binding, LoonClick)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bind("${position + 1}")
    }

    override fun getItemCount(): Int {
        return 49
    }

    inner class ViewHolder(private val binding: AdapterAddmynumBinding, val onClick: (String) -> Unit) : RecyclerView.ViewHolder(binding.root) {
        init {
            binding.adapterAddmynumCheck.setOnClickListener {
                binding.adapterAddmynumCheck.apply {
                    isCheckButtonSelected = !isCheckButtonSelected
                    onClick(numberString)
                }
            }
        }


        fun bind(number: String) {
            val checkButton = binding.adapterAddmynumCheck

            when {
                number.toInt() == 49 -> {
                    checkButton.visibility = View.VISIBLE
                    checkButton.numberString = "자동"
                    checkButton.binding.checkText.textSize = checkButton.binding.checkText.textSize * 0.3f
                }
                number.toInt() > 45 -> {
                    checkButton.visibility = View.INVISIBLE
                }
                else -> {
                    checkButton.numberString = number
                }
            }
        }
    }
}
```

```kotlin
private fun updateUI() {
    recyclerView.adapter = AddMyNumAdapter {
        adapterOnClick(it)
    }
}

private fun adapterOnClick(numberString: String) {
    Logger.v(numberString)
}
```

[android - Kotiln: pass data from adapter to activity - Stack Overflow](https://stackoverflow.com/questions/56826346/kotiln-pass-data-from-adapter-to-activity)
[views-widgets-samples/FlowersListActivity.kt at main · android/views-widgets-samples · GitHub](https://github.com/android/views-widgets-samples/blob/main/RecyclerViewKotlin/app/src/main/java/com/example/recyclersample/flowerList/FlowersListActivity.kt)

