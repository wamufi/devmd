---
title: 에러 핸들링
date:  2025-03-12
time:  05:38:34
tags:
  - kotlin
  - android
  - retrofit
---
ApiResult.kt
```kotlin
import kotlinx.coroutines.flow.Flow  
import kotlinx.coroutines.flow.flow  
import retrofit2.Response  
  
sealed class ApiResult<out T> {  
    data object Loading : ApiResult<Nothing>()  
    data class Success<out T>(val data: T) : ApiResult<T>()  
    data class Error(val code: Int? = null, val message: String? = null, val throwable: Throwable? = null) : ApiResult<Nothing>()
}  
  
fun <T : Any> safeApiFlow(execute: suspend () -> Response<T>): Flow<ApiResult<T>> = flow {  
    emit(ApiResult.Loading)  
    try {  
        val response = execute.invoke()  
        if (response.isSuccessful) {  
            response.body()?.let { emit(ApiResult.Success(it)) }  
                ?: emit(ApiResult.Error(code = response.code(), message = response.message()))  
        } else {  
            emit(ApiResult.Error(code = response.code(), message = response.message()))  
        }    } catch (e: Exception) {  
        emit(ApiResult.Error(message = e.message, throwable = e))  
    }}  
  
suspend fun <T> safeApiCall(execute: suspend() -> Response<T>): ApiResult<T> {  
    return try {  
        val response = execute.invoke()  
        if (response.isSuccessful) {  
            response.body()?.let { ApiResult.Success(it) }  
                ?: ApiResult.Error(code = response.code(), message = response.message())  
        } else {  
            ApiResult.Error(code = response.code(), message = response.message())  
        }    } catch (e: Exception) {  
        ApiResult.Error(throwable = e)  
    }}
```

Repository.kt
```kotlin
class LoginRepository @Inject constructor(private val service: ApiService) {
    suspend fun login(parameter: LoginRequest): ApiResult<LoginResponse> {
        return safeApiCall { service.login(parameter) }
    }
}
```

ViewModel.kt
```kotlin
@HiltViewModel
class LoginViewModel @Inject constructor(private val repository: LoginRepository) : ViewModel() {
    private val _uiState: MutableStateFlow<ApiResult<LoginResponse>> = MutableStateFlow(ApiResult.Loading)
    val uiState = _uiState.asStateFlow()

    fun login(username: String, password: String) {
        val parameter = LoginRequest(username, password)
        viewModelScope.launch {
            _uiState.value = repository.login(parameter)
        }
    }
}
```

Screen.kt
```kotlin
@Composable
fun LoginScreen(viewModel: LoginViewModel = hiltViewModel()) {
    val uiState by viewModel.uiState.collctAsState()
    when (uiState) {
        is ApiResult.Loading -> { CircularProgressIndicator() }
        is ApiResult.Success -> {
            val loginResponse = (uiState as ApiResult.Success).data
        }
        is ApiResult.Error -> {
            val error = (uiState as ApiResult.Error)
        }
    }
}
```