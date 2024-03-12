---
title: Date 다루기
date: 2024-03-11
time: 10:01:34
tags:
- android
- kotlin
---
## 현재 날짜 가져오기
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

## 특정 날짜 가져오기
```kotlin
// 오늘의 시작
val startOfDay = Calendar.getInstance().apply {
	set(Calendar.HOUR_OF_DAY, 0)
	set(Calendar.MINUTE, 0)
	set(Calendar.SECOND, 0)
	set(Calendar.MILLISECOND, 0)
}.time // Tue Mar 11 00:00:00 GMT+09:00 2024

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
	val startOfDay1 = LocalDate.now().atStartOfDay() // 2024-03-11T00:00
	val startOfDay2 = LocalDate.now().atTime(LocalTime.MIN) // 2024-03-11T00:00
	val startOfDay3 = LocalTime.MIN.atDate(LocalDate.now()) // 2024-03-11T00:00
	val startOfDay4 = LocalDateTime.now().with(LocalTime.MIN) // 2024-03-11T00:00
}

// 오늘의 마지막
val endOfDay = Calendar.getInstance().apply {
	set(Calendar.HOUR_OF_DAY, 23)
	set(Calendar.MINUTE, 59)
	set(Calendar.SECOND, 59)
} // Tue Mar 11 23:59:59 GMT+09:00 2024

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
	val endOfDay1 = LocalDate.now().atTime(LocalTime.MAX) // 2024-03-12T23:59:59.999999999
	val endOfDay2 = LocalTime.MAX.atDate(LocalDate.now()) // 2024-03-12T23:59:59.999999999
	val endOfDay3 = LocalDateTime.now().with(LocalTime.MAX) // 2024-03-12T23:59:59.999999999
}
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