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
val calendar = Calendar.getInstance().time // Mon Mar 11 23:38:32 GMT+09:00 2024
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    val localDateTime = LocalDateTime.now() // 2024-03-11T23:38:32.672406
}
val date = Date() // Mon Mar 11 23:38:32 GMT+09:00 2024
val currentTime = {  
    val millis = System.currentTimeMillis()  
    val dateFormat = SimpleDateFormat("yyyy-MM-dd hh:mm:ss")  
    dateFormat.format(millis)  
}() // 2024-03-11 23:38:32
```

## String to Local date format string
```kotlin
fun String.toLocalDateString(): String {
	val simpleDateFormat = SimpleDateFormat("yyyy-MM-dd HH:mm:ss") // 2024-03-12 08:00:00
	val date = simpleDateFormat.parse(this)  
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT)  
	return dateFormat.format(date) // Mar 12, 2024 8:00 AM
}
```

## Date to Local date format string
```kotlin
fun Date.toLocalDateString(): String { // Tue Mar 12 08:00:00 GMT+09:00 2024
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT)  
	return dateFormat.format(this) // Mar 12, 2024 8:00 AM
}
```

https://madplay.github.io/post/reasons-why-javas-date-and-calendar-was-bad