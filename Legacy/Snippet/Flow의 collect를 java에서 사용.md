Flow의 collect를 java에서 사용
---
.kt
```kotlin
interface FlowCallback<T> {
    fun onResult(result: T): Long
}

fun getUserPhoneBlockDate(sch_ph: String, preFlag: String, flowCallback: FlowCallback<Long>) {
    CoroutineScope(ioDispatcher).launch {
        repository.getUserPhoneBlockDate(sch_ph, preFlag).collect {
            flowCallback.onResult(it)
        }
    }
}
```
.java
```java
new UserPhoneBlockUseCase().getUserPhoneBlockDate(sch_ph, preFlag, new UserPhoneBlockUseCase.FlowCallback<Long>() {
    @Override
    public Long onResult(Long result) {
        return result;
    }
});
```

급하게 대충 한 거라 에러는 나지만 일단은 이렇게 사용

https://stackoverflow.com/questions/60605176/kotlin-flows-java-interop-callback