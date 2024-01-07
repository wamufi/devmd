---
boostnote:
  createdAt: '2019-02-13T02:13:41.713Z'
  updatedAt: '2019-03-06T01:33:13.868Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Toolbar에 back button 추가
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: 70881c73-8531-4036-9015-ed224bff4311
  storage: d112f8ec1e85e056a09d
---

Toolbar에 back button 추가
---
[Android Notes 24 : How to add Back Button at Toolbar [Updated]](https://freakycoder.com/android-notes-24-how-to-add-back-button-at-toolbar-941e6577418e)

1. Toolbar 초기화
```java
Toolbar toolbar = findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
getSupportActionBar().setDisplayHomeAsUpEnabled(true);
```
2. 버튼 눌렀을 때 동작
```java
@Override
public boolean onSupportNavigateUp() {
    onBackPressed();
    return true;
}
```
또는
```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();
    
    if (id == android.R.id.home) {
        onBackPressed();
        return true;
    }
    
    return super.onOptionsItemSelected(item);
}
```
버튼이 여러개 있으면 하단 방법이 더 좋을 듯

`AndroidManifest.xml`에 `android:parentActivityName="com.example.myfirstapp.MainActivity"` 추가 vs `onBackPressed()` 다르게 동작한다.