---
boostnote:
  createdAt: '2019-01-15T02:48:29.783Z'
  updatedAt: '2019-03-06T01:33:31.333Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: GradientDrawable
  tags:
    - Android
    - UI
  isStarred: false
  linesHighlighted: []
  key: 2f185800-0ac4-4036-ade6-0fbe5bf0fd69
  storage: d112f8ec1e85e056a09d
---

GradientDrawable
---
```java
TextView platinumText = findViewById(R.id.text_platinum);
GradientDrawable roundView = (GradientDrawable) platinumText.getBackground();
roundView.setColor(getResources().getColor(android.R.color.transparent));
roundView.setStroke(3, getResources().getColor(R.color.greyTwo));
platinumText.setBackground(roundView);
```

https://stackoverflow.com/questions/4772537/i-need-to-change-the-stroke-color-to-a-user-defined-color-nothing-to-do-with-th