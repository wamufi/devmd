---
boostnote:
  createdAt: '2019-01-17T01:49:53.695Z'
  updatedAt: '2021-08-11T06:57:07.479Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Button 선택 후 같은 자식 button들 deselect
  tags:
    - Android
    - Java
    - Kotlin
  isStarred: false
  linesHighlighted: []
  key: e5fef575-01f4-438e-aff1-d20ec93bef01
  storage: d112f8ec1e85e056a09d
---

Button 선택 후 같은 자식 button들 deselect
---
```kotlin
// 선택한 버튼만 select, 다른 버튼 deselect
private fun setSelectedButton(button: Button) {
    val layout = button.parent as? ViewGroup
    layout?.forEach {
        val deselectButton = it as? Button
        deselectButton?.isSelected = false
    }

    button.isSelected = true
}
```

```kotlin
// 같은 레이아웃에 있는 모든 버튼 deselect
private fun setDeselectedButton(button: Button) {
    val layout = button.parent as? ViewGroup
    layout?.forEach {
        val deselectButton = it as? Button
        deselectButton?.isSelected = false
    }
}
```

```java
private void selectDateText(TextView textView) {
    ViewGroup parentView = (ViewGroup) textView.getParent();

    for (int i = 0; i < parentView.getChildCount(); i++) {
        TextView childTextView = (TextView) parentView.getChildAt(i);
        childTextView.setTextColor(getResources().getColor(R.color.textColor));
        childTextView.setTypeface(Typeface.create(childTextView.getTypeface(), Typeface.NORMAL));
    }

    textView.setTypeface(textView.getTypeface(), Typeface.BOLD);
    textView.setTextColor(getResources().getColor(R.color.brownOne));
}
```

```java
private void selectPriceButton(Button currentButton) {
    for (int i = 0; i < chargeTable.getChildCount(); i++) {
        TableRow row = (TableRow) chargeTable.getChildAt(i);
        for (int j = 0; j < row.getChildCount(); j++) {
            Button button = (Button) row.getChildAt(j);
            button.setSelected(false);
        }
    }

    currentButton.setSelected(true);
    setStateChargeSetButton();
}
```

```java
private void selectPayButton(Button currentButton) {
    LinearLayout payLinear = getView().findViewById(R.id.pay_linear);
    for (int i = 0; i < payLinear.getChildCount(); i++) {
        Button button = (Button) payLinear.getChildAt(i);
        button.setSelected(false);
    }

    currentButton.setSelected(true);
    setStateEcardPresentButton();
}
```
