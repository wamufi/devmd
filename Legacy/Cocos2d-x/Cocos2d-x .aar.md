---
boostnote:
  createdAt: '2021-10-14T03:37:48.832Z'
  updatedAt: '2021-10-27T05:05:46.478Z'
  type: MARKDOWN_NOTE
  folder: 4d5ef4df7aca689c0c48
  title: Cocos2d-x .aar
  tags:
    - Cocos2d-x
    - Android
    - aar
  linesHighlighted: []
  isStarred: false
  key: 96fe3c62-0c3b-4de6-ab28-f23a1d46ddab
  storage: d112f8ec1e85e056a09d
---

Cocos2d-x .aar
---
[Cocos2d-x 4.0, 4.x Mac Android Setup (Android Studio) - Game Development, Mobile Programming - YouTube](https://www.youtube.com/watch?v=veBSTU0c2YY)
세팅 방법은 위 링크 참조

프로젝트 이름은 CocosTest
패키지명은 com.asdf.cocostest

1. apply plugin: 'com.android.library' 추가
applicationId, shirinkResources, android.applicationVariants.all 주석
build.gradle (Module: proj.android.CocosTest)
```
//apply plugin: 'com.android.application'
apply plugin: 'com.android.library'

android {
    defaultConfig {
//        applicationId "com.asdf.cocostest"
        ...
    }
    
    buildTypes {
        release {
            ...
//            shirinkResources true
            ...
        }
    }
}

//android.applicationVariants.all { variant ->
//    ...
//}
```

2. aar 추출

`./gradlew assembleRelease`

3. App에서 콜
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val button = findViewById<Button>(R.id.button)
        button.setOnClickListener {
            val intent = Intent(Intent.ACTION_VIEW)
            intent.component = ComponentName("com.asdf.cocostest", "org.cocos2dx.cpp.AppActivity")
            startActivity(intent)
        }
    }
}
```
