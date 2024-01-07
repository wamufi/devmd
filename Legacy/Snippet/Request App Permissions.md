---
boostnote:
  createdAt: '2019-02-01T04:07:10.628Z'
  updatedAt: '2019-03-06T01:33:17.952Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Request App Permissions
  tags:
    - Android
  linesHighlighted: []
  isStarred: false
  key: 8d6f4708-c933-41c5-859d-c5daf9a98727
  storage: d112f8ec1e85e056a09d
---

Request App Permissions
---
[Request App Permissions  \|  Android Developers](https://developer.android.com/training/permissions/requesting#java)

AndroidManifest.xml
```xml
<manifest>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
</manifest>
```

MainActivity.java
```java
private void checkStoragePermission() {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
        if (checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED
                || checkSelfPermission(Manifest.permission.READ_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
            Log.v("Storage", "checkStoragePermission() - requestPermissions");
            requestPermissions(new String[]{Manifest.permission.READ_EXTERNAL_STORAGE, Manifest.permission.WRITE_EXTERNAL_STORAGE}, 1000);
        } else {
            Log.v("Storage", "checkStoragePermission()");
            setDeviceForFingerPush();
        }
    } else {
        setDeviceForFingerPush();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    switch (requestCode) {
        case 1000: // checkStoragePermission
            Log.v("Storage", "onRequestPermissionsResult()");
            setDeviceForFingerPush();
            break;
    }
}
```