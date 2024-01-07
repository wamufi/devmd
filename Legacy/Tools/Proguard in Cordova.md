---
boostnote:
  createdAt: '2020-04-27T07:47:25.968Z'
  updatedAt: '2020-08-28T02:17:52.346Z'
  type: MARKDOWN_NOTE
  folder: ec2c5e1a46e8f816e4bb
  title: Proguard in Cordova
  tags:
    - Android
    - Cordova
    - Proguard
  linesHighlighted: []
  isStarred: false
  key: 564484b2-6c12-47cb-8de8-7b174e69e4ba
  storage: d112f8ec1e85e056a09d
---

Proguard in Cordova
===
1. install cordova-plugin-proguard
`cordova plugin add cordova-plugin-proguard`
[cordova-plugin-proguard - npm](https://www.npmjs.com/package/cordova-plugin-proguard)

2. build release
`cordova build android --release`
    - works only in release
    - `
R8: The rule '-keep public class * extends androidx.versionedparcelable.VersionedParcelable {
  <init>();
}'' uses extends but actually matches implements.`
에러 발생 시에는 Android Studio에서 Signed APK로 할 것


3. decompile apk
[Decompile .apk](:note:b5a7efe7-2c20-450a-b741-dc078080463e)

4. custom proguard
    - file location
    `.../platforms/android/platform_www/proguard-custom.txt`