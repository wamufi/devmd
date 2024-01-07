---
boostnote:
  createdAt: '2022-02-13T10:50:49.639Z'
  updatedAt: '2022-02-13T10:53:37.094Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'BuildConfig, properties'
  tags:
    - Kotlin
    - Android
  linesHighlighted: []
  isStarred: false
  key: 716c03ca-ee0f-4e62-8241-201038f53a53
  storage: d112f8ec1e85e056a09d
---

BuildConfig, properties
---
ServiceKey 등을 숨겨야 할 때 사용

build.gradle (app)
```groovy
def properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

android {
    ...
    
    defaultConfig {
        ...
        
        buildConfigField "String", "SERVICE_KEY", properties['service_key']
    }
}
```

local.properties
```groovy
service_key="seUGpp3xcB0kAY09ZmZqw8zZB8Jd0tG/R7N1Cipo/D/0HwKemq9efoVd+LT2+7FSfJYVb8wyz4stz90WV6IhEg=="
```

사용
```kotlin
import com.wamufi.airpollution.BuildConfig


private const val SERVICE_KEY = BuildConfig.SERVICE_KEY
```