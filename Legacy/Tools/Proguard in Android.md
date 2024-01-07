---
boostnote:
  createdAt: '2020-04-27T08:20:13.762Z'
  updatedAt: '2020-07-14T00:29:23.271Z'
  type: MARKDOWN_NOTE
  folder: ec2c5e1a46e8f816e4bb
  title: Proguard in Android
  tags:
    - Android
    - Proguard
  linesHighlighted: []
  isStarred: false
  key: e72e44de-b4b4-443a-b1e2-f1002f00bc10
  storage: d112f8ec1e85e056a09d
---

Proguard in Android
===
build.gradle
```
android {
    buildTypes {
        release {
            minifyEnabled true // 코드 축소
            shrinkResources true // 리소스 축소
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            }
        }
        ...
    }

```

*Execution failed for task ':app:transformClassesAndResourcesWithProguardForRelease'.
-ignorewarnings*
```
### Execution failed for task ':app:transformClassesAndResourcesWithProguardForRelease'.
-ignorewarnings
-keep class * {
    public private *;
}
```

[Shrink, obfuscate, and optimize your app  \|  Android Developers](https://developer.android.com/studio/build/shrink-code?hl=ko)
[안드로이드 앱 난독화(Proguard & Dexguard)를 해보자! 1. 프로가드 편 - $IC’s LAB - Medium](https://medium.com/ics-lab/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%B1-%EB%82%9C%EB%8F%85%ED%99%94-proguard-dexguard-%EB%A5%BC-%ED%95%B4%EB%B3%B4%EC%9E%90-1-%ED%94%84%EB%A1%9C%EA%B0%80%EB%93%9C-%ED%8E%B8-799999a43c4d)
[Android Proguard 사용하기 - woong's](https://iw90.tistory.com/298)