---
title: Preferences DataStore
date:  2025-02-11
time:  09:36:09
tags:
  - android
  - kotlin
  - compose
  - datastore
---
## Preferences DataStore
Preferences DataStore는 키를 사용하여 데이터를 저장하고 데이터에 액세스합니다. 이 구현은 유형 안전성을 제공하지 않으며 사전 정의된 스키마가 필요하지 않습니다.

AppModule.kt
```kotlin
private val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "viewerPreferences")  
  
@Module  
@InstallIn(SingletonComponent::class)  
object AppModule {  
    @Provides  
    fun provideViewerPreferencesRepository(@ApplicationContext context: Context): ViewerPreferencesRepository {  
        return ViewerPreferencesRepository(context.dataStore)  
    }
}
```

ViewerPreferencesRepository.kt
```kotlin
data class ViewerPreferences(val fontScale: Float, val lineHeight: Float, val fontFamily: FontFamily)

class ViewerPreferencesRepository @Inject constructor(private val dataStore: DataStore<Preferences>) {  
  
    private object PreferencesKeys {  
        val FONT_SCALE = floatPreferencesKey("font_scale")  
        val LINE_HEIGHT = floatPreferencesKey("line_height")  
        val FONT_FAMILY = stringPreferencesKey("font_family")  
    }  
    val viewerPreferencesFlow: Flow<ViewerPreferences> = dataStore.data.map { preferences ->  
        mapViewerPreferences(preferences)  
    }  
  
    suspend fun updateFontScale(fontScale: Float) {  
        dataStore.edit { preferences ->  
            preferences[PreferencesKeys.FONT_SCALE] = fontScale  
        }  
    }  
  
    suspend fun updateLineHeight(lineHeight: Float) {  
        dataStore.edit { preferences ->  
            preferences[PreferencesKeys.LINE_HEIGHT] = lineHeight  
        }  
    }  
  
    suspend fun updateFontFamily(fontFamily: String) {  
        dataStore.edit { preferences ->  
            preferences[PreferencesKeys.FONT_FAMILY] = fontFamily  
        }  
    }  
  
    suspend fun fetchInitialPreferences() = mapViewerPreferences(dataStore.data.first().toPreferences())  
  
    private fun mapViewerPreferences(preferences: Preferences): ViewerPreferences {  
        val fontScale = preferences[PreferencesKeys.FONT_SCALE] ?: 1.0f  
        val lineHeight = preferences[PreferencesKeys.LINE_HEIGHT] ?: 1.0f  
        val fontFamily = ViewerFont.entries.find { it.name == preferences[PreferencesKeys.FONT_FAMILY] } ?: ViewerFont.ORIGINAL  
  
        return ViewerPreferences(fontScale, lineHeight, fontFamily.fontFamily)  
    }
}
```

ViewerViewModel.kt
```kotlin
@HiltViewModel
class ViewerViewModel @Inject constructor(private val viewerPreferencesRepository: ViewerPreferencesRepository) : ViewModel() {
	private val _viewerPreferencesFlow = viewerPreferencesRepository.viewerPreferencesFlow  
	  
	var fontScale: Float by mutableFloatStateOf(1.0f)  
	var lineHeight: Float by mutableFloatStateOf(1.0f)  
	var fontFamily: FontFamily by mutableStateOf(ViewerFont.ORIGINAL.fontFamily)  
	  
	private var debounceTime = 500L

	fun updateFontScale(value: Float) {  
	    fontScale = value  
	  
	    viewModelScope.launch {  
	        delay(debounceTime)  
	        if (isActive) {  
	            viewerPreferencesRepository.updateFontScale(fontScale)  
	        }
	    }  
	}  
  
	fun updateFontFamily(font: ViewerFont) {  
	    fontFamily = font.fontFamily  
	  
	    viewModelScope.launch {  
	        delay(debounceTime)  
	        if (isActive) {  
	            viewerPreferencesRepository.updateFontFamily(font.name)  
	        }    }  
	}  
	  
	fun updateLineHeight(value: Float) {  
	    lineHeight = value  
	  
	    viewModelScope.launch {  
	        delay(debounceTime)  
	        if (isActive) {  
	            viewerPreferencesRepository.updateLineHeight(lineHeight)  
	        }
	    }  
	}
}
```

ThemeScreen.kt
```kotlin
@Composable
fun ThemeScreen(viewModel: ViewerViewModel) {
	// ...
	Slider(value = viewModel.fontScale, onValueCahange = { viewModel.updateFontScale(it) })
}
```