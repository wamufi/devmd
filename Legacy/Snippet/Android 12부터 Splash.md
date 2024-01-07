---
boostnote:
  createdAt: '2022-03-18T08:57:40.264Z'
  updatedAt: '2022-03-18T09:02:25.074Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Android 12부터 Splash
  tags:
    - Android
    - Kotlin
    - Splash
  linesHighlighted: []
  isStarred: false
  key: df124f76-9ad7-4528-b507-9af3fd777364
  storage: d112f8ec1e85e056a09d
---

Android 12부터 Splash
---
아이콘 나오는 스플래시 -> 브랜딩 이미지 띄우는 스플래시 액티비티 -> 메인 액티비티

themes.xml
```xml
<style name="Theme.Lotto.Starting" parent="Theme.SplashScreen">
    <item name="windowSplashScreenBackground">?attr/backgroundColor</item>
    <item name="windowSplashScreenAnimatedIcon">@mipmap/ic_launcher_foreground</item>
    <item name="windowSplashScreenAnimationDuration">500</item>
    <item name="postSplashScreenTheme">@style/Theme.Lotto.Splash</item>
</style>

<style name="Theme.Lotto.Splash" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="android:statusBarColor">@color/splash_background</item>
    <item name="android:navigationBarColor">@color/splash_background</item>
</style>
```
AndroidManifest.xml
```xml
<application
    ...
    android:theme="@style/Theme.Lotto.Starting">
    <activity
        android:name=".SplashActivity"
        android:exported="true"
        android:theme="@style/Theme.Lotto.Splash"
        android:label="@string/app_name"
        android:noHistory="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
</application>
```

SplashActivity.kt
```kotlin
class SplashActivity : AppCompatActivity() {

    private lateinit var mContext: Context

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_splash)

        val handler = Handler()
        handler.postDelayed({ startHomeActivity() }, 1000)
    }

    private fun startHomeActivity() {
        val intent = Intent(this, MainActivity::class.java)
        startActivity(intent)
    }
}
```