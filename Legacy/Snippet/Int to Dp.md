---
boostnote:
  createdAt: '2019-01-18T01:34:18.144Z'
  updatedAt: '2019-03-06T01:33:26.731Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Int to Dp
  tags:
    - Android
  isStarred: false
  linesHighlighted: []
  key: e7e1ac04-c7e5-487c-bad8-bb1e6d8ecf2c
  storage: d112f8ec1e85e056a09d
---

Int to Dp
---
```java
private int calcIntToDp(int dp) {
    final float scale = getContext().getResources().getDisplayMetrics().density;
    int pixels = (int) (dp * scale + 0.5f);

    return pixels;
}
```

```java
int pixels = (int) (dp * getContext().getResources().getDisplayMetrics().density + 0.5f);
```