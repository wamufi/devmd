---
title: Date 다루기
date: 2024-03-11
time: 10:01:34
tags:
- android
- kotlin
---
## String to Local date format string
2024-03-11 10:01:34를 한국식으로 표현
```kotlin
fun String.fromStringToLocalDateString(): String {
	val simpleDateFormat = SimpleDateFormat("yyyy-MM-dd HH:mm:ss")  
	val date = simpleDateFormat.parse(this)  
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT, Locale.KOREA)  
	return dateFormat.format(date)
}
```

## Local date format to String
```kotlin
fun Date.toLocalDateString(): String {
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT, Locale.KOREA)  
	return dateFormat.format(this)
}
```