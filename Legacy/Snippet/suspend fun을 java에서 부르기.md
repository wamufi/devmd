suspend fun을 java에서 부르기
---
test.kt
```kotlin
// 사용할 suspend fun
private suspend fun getCount(): Int {
    return repository.getCount()
}

// 1. CompletableFuture 사용
fun getCountFuture(): CompletableFuture<Int> = CoroutineScope(Dispatchers.IO).future {
    getCount()
}

// 2. async 사용
fun getCountAsync(): Int {
    var result = 0
    CoroutineScope(Dispachters.IO).launch {
        val waitFor = async {
            result = getCount()
        }
        waitFor.await()
    }

    return result
}
```

test.java
```java
// 1. CompletableFuture 사용
// join() 때문에 API 24 이상 사용 가능
// get()도 사용 가능: try-catch 구문 필요
int countFuture = new test().getCountFuture().join();

// 2. async 사용
// result를 나중에 부름... 아마 사용 못할 듯?
int countAsync = new test().getCountAsync();
```