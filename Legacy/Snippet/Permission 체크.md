---
boostnote:
  createdAt: '2018-01-24T08:02:35.654Z'
  updatedAt: '2019-03-06T01:33:47.290Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Permission 체크
  tags:
    - Android
    - Permission
  isStarred: false
  linesHighlighted: []
  key: 46042fcac1c395117e4a
  storage: d112f8ec1e85e056a09d
---

Permission 체크
---
Fragment
```java
private void checkForPermissions() {
    if (ContextCompat.checkSelfPermission(getActivity().getApplicationContext(), Manifest.permission.RECEIVE_SMS) != PackageManager.PERMISSION_GRANTED
            || ContextCompat.checkSelfPermission(getActivity().getApplicationContext(), Manifest.permission.PROCESS_OUTGOING_CALLS) != PackageManager.PERMISSION_GRANTED) {
        requestPermissions(new String[]{ Manifest.permission.RECEIVE_SMS, Manifest.permission.PROCESS_OUTGOING_CALLS }, 1);
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    if (requestCode == 1) {
        if (grantResults.length == 1 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            Snackbar.make(mLayout, "Permission was granted.", Snackbar.LENGTH_SHORT).show();
        } else {
            Snackbar.make(mLayout, "Permission was NOT granted.", Snackbar.LENGTH_SHORT).show();
        }
    } else {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```