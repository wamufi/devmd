---
title: Date 다루기
date: 2024-03-11
time: 10:01:34
tags:
- android
- kotlin
---
## 현재 날짜
```kotlin
val date = Date() // Mon Mar 11 23:38:32 GMT+09:00 2024
val calendar = Calendar.getInstance().time // Mon Mar 11 23:38:32 GMT+09:00 2024
```
## String to Local date format string
2024-03-11 10:01:34를 한국식으로 표현
```kotlin
fun String.toLocalDateString(): String {
	val simpleDateFormat = SimpleDateFormat("yyyy-MM-dd HH:mm:ss") // 2024-03-12 08:00:00
	val date = simpleDateFormat.parse(this)  
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT, Locale.KOREA)  
	return dateFormat.format(date) // 2024. 3. 12. 오전 8:00
}
```

## Date to Local date format string
```kotlin
fun Date.toLocalDateString(): String { // Tue Mar 12 08:00:00 GMT+09:00 2024
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT, Locale.KOREA)  
	return dateFormat.format(this) // 2024. 3. 12. 오전 8:00
}
```