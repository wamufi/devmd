---
boostnote:
  createdAt: '2021-08-26T09:59:53.222Z'
  updatedAt: '2021-10-27T05:05:52.606Z'
  type: MARKDOWN_NOTE
  folder: f9b4c2c416b7c9c01097
  title: How to Create Multiple Tappable Links in a UILabel \| @samwize
  tags: []
  linesHighlighted: []
  isStarred: false
  key: 0c35dcf0-5c2f-4939-ae6e-87e49d6c95fa
  storage: d112f8ec1e85e056a09d
---

[How to Create Multiple Tappable Links in a UILabel \| @samwize](https://samwize.com/2016/03/04/how-to-create-multiple-tappable-links-in-a-uilabel/)

```kotlin
private fun setTermsText(){
        val sequence = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            Html.fromHtml(getString(R.string.terms_agreement_check),Html.FROM_HTML_MODE_LEGACY)
        } else {
            Html.fromHtml(getString(R.string.terms_agreement_check))
        }
        val stringBuilder = SpannableStringBuilder(sequence)
        val underlineSpan = stringBuilder.getSpans(0, stringBuilder.length, UnderlineSpan::class.java)

        underlineSpan.forEachIndexed { index, underlineSpan ->
            val start = stringBuilder.getSpanStart(underlineSpan)
            val end = stringBuilder.getSpanEnd(underlineSpan)
            val flags = stringBuilder.getSpanFlags(underlineSpan)
            val activityLauncher = object: ClickableSpan() {
                override fun onClick(widget: View) {
                    if(index == 0){
                        startTermsAndConditions()
                    } else {
                        startPolicy()
                    }
                }
            }
            stringBuilder.setSpan(activityLauncher, start, end, flags)
            stringBuilder.setSpan(ForegroundColorSpan(ContextCompat.getColor(this, R.color.skyblue)), start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE)
        }

        cm_signup_term_text.text = stringBuilder
        cm_signup_term_text.movementMethod = LinkMovementMethod.getInstance()
    }
```