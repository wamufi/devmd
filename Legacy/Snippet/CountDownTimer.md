---
boostnote:
  createdAt: '2021-08-02T03:40:22.275Z'
  updatedAt: '2021-08-02T03:45:52.311Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: CountDownTimer
  tags:
    - Kotlin
    - Android
    - Timer
  linesHighlighted: []
  isStarred: false
  key: 15818e7d-2af4-4a8f-a5fb-adcb6e31f75a
  storage: d112f8ec1e85e056a09d
---

CountDownTimer
---
인증 요청하고 시간 카운트다운 할 때 사용
AsyncTask라던가 Thread 안 써도 된다.

시간 계산은 Java의 TimeUnit을 쓸 수도, 그냥 계산할 수도 있다.

출력은 "0:00"
%02d의 0은 붙일 숫자, 2는 자릿수, d는 정수

```kotlin
private fun startCountDownTimer() {
    val sec: Long = 1000 // 1s == 1000ms
    val millis: Long = 3 * 60 * sec // 3m = 3 * 60s * 1000ms
    countDownTimer = object : CountDownTimer(millis, sec) {
        override fun onTick(millisUntilFinished: Long) {
            val minutes = TimeUnit.MILLISECONDS.toMinutes(millisUntilFinished)
            val seconds = TimeUnit.MILLISECONDS.toSeconds(millisUntilFinished) - TimeUnit.MINUTES.toSeconds(minutes)

//            val minutes = (millisUntilFinished / sec) / 60
//            val seconds = (millisUntilFinished / sec) % 60

            cm_code_expires.text = String.format("%d:%02d", minutes, seconds)
        }

        override fun onFinish() {
            cm_code_expires.text = "0:00"
        }
    }.start()
}
```

[time - How to convert Milliseconds to "X mins, x seconds" in Java? - Stack Overflow](https://stackoverflow.com/questions/625433/how-to-convert-milliseconds-to-x-mins-x-seconds-in-java)
[print("%02d" ~) 뜻](https://curious23.tistory.com/146)
