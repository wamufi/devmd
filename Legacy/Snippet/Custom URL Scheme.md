---
boostnote:
  createdAt: '2019-02-18T05:53:29.054Z'
  updatedAt: '2019-03-06T01:33:07.276Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Custom URL Scheme
  tags:
    - Android
  linesHighlighted: []
  isStarred: false
  key: de8d4f2a-c67d-481c-92fe-4ac9aa07a2de
  storage: d112f8ec1e85e056a09d
---

Custom URL Scheme
---
```java
try {
    Intent intent = new Intent();
    intent.setAction(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("maeildo://"));
    startActivity(intent);
} catch (ActivityNotFoundException e) {
    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("market://details?id=com.maeil.point"));
    startActivity(intent);
}
```