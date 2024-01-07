Coroutine with Room and Retrofit
---
https://developer.android.com/codelabs/kotlin-coroutines#8
https://sandn.tistory.com/94

Retrofit will automatically make suspend functions main-safe so you can call them directly from Dispatchers.Main.

Both Room and Retrofit make suspending functions main-safe.
It's safe to call these suspend funs from Dispatchers.Main, even though they fetch from the network and write to the database.

Both Room and Retrofit use a custom dispatcher and do not use Dispatchers.IO.
Room will run coroutines using the default query and transaction Executor that's configured.
Retrofit will create a new Call object under the hood, and call enqueue on it to send the request asynchronously.

Even better, we got rid of the withContext. Since both Room and Retrofit provide main-safe suspending functions, it's safe to orchestrate this async work from Dispatchers.Main.

You do not need to use withContext to call main-safe suspending functions.
By convention, you should ensure that suspend functions written in your application are main-safe. That way it is safe to call them from any dispatcher, even Dispatchers.Main.

We consider a function main-safe when it doesn't block UI updates on the main thread.

Retrofit과 Room을 사용할 때는 Dispatchers.IO를 쓰지 말고 그냥 다이렉트로 Dispatchers.Main에서 사용하라는 얘기.
자체적으로 돌아가는 게 커스텀 디스패처가 있는 것 같다. withContext도 쓸 필요가 없다고.

그렇다고 Activity, Fragment에서 그냥 다이렉트로 우다다 부르면 Cannot access main thread 어쩌구저쩌구 IllegalException에러 발생함.  뭐야 다른건가? 나의 공부가 부족한 탓이다...


* 참고사항
ViewModel(Main): Flow 사용 
- Flow 미사용 + suspend, viewModelScope 사용 안됨
- Flow만 미사용 시 main thread에서 db access 에러 발생
Util(NOT Main): Flow 미사용
