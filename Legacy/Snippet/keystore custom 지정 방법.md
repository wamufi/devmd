---
boostnote:
  createdAt: '2021-02-01T07:32:42.651Z'
  updatedAt: '2021-02-01T07:33:57.348Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: keystore custom 지정 방법
  tags:
    - Android
    - AndroidStudio
    - Gradle
  linesHighlighted: []
  isStarred: false
  key: 72748b37-ed3b-4651-8ac6-7341276e2ad7
  storage: d112f8ec1e85e056a09d
---

keystore custom 지정 방법
---
app/build.gradle
```
android {
    signingConfigs {
        debug {
            storeFile file("../keystore/debug.keystore")
            storePassword "took123"
            keyAlias "took"
            keyPassword "took123"
        }
    }
}
```