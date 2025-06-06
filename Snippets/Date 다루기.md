---
title: Date 다루기
date: 2024-03-11
time: 10:01:34
tags:
- android
- kotlin
- date
---
## java.time API에서 시간, 날짜를 다루는 클래스
|**클래스**|**포함 정보**|**타임존 처리**|**사용 예시**|
|---|---|---|---|
|LocalDateTime|날짜 + 시간 (년·월·일·시·분·초)|없음 (순수 날짜·시간)|“2025-05-14T14:30:00” 같은, 타임존 무관 값|
|OffsetDateTime|날짜 + 시간 + UTC 오프셋 (±hh:mm)|고정된 오프셋만 표현 (예: +09:00)|“2025-05-14T14:30:00+09:00” 같은, 오프셋 기반|
|ZonedDateTime|날짜 + 시간 + 타임존 ID (Region)|지역(zone) 단위까지 완전 표현|“2025-05-14T14:30:00[Asia/Seoul]” 같은, DST 등 고려|
|Instant|타임라인상의 한 점 (UTC 기준)|UTC 기준의 절대 시점만|내부 저장·비교용, “2025-05-14T05:30:00Z” 같은|

- **타임존 무관** 스케줄 표현 → LocalDateTime
- **고정 오프셋(±hh:mm)** 사용 API → OffsetDateTime
- **지역 기반 정확한 타임존(DST 포함)** → ZonedDateTime
- **절대 비교·저장** → Instant

## 현재 날짜, 시간 가져오기
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

## 특정 날짜, 시간 가져오기
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

// 내일 오전 8시
val tomorrowAm = Calendar.getInstance().apply {  
    add(Calendar.DAY_OF_MONTH, 1)  
    set(Calendar.HOUR_OF_DAY, 8)  
    set(Calendar.MINUTE, 0)  
    set(Calendar.SECOND, 0)  
}.time // Wed Mar 12 08:00:00 GMT+09:00 2024

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    val tomorrowAm = LocalDateTime.now().plusDays(1).withHour(8).withMinute(0).withSecond(0).withNano(0) // 2024-03-12T08:00
}
```

## ZonedDateTime
```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
	// LocalDateTime to ZonedDateTime
	val nowAtHelsinki = LocalDateTime.now().atZone(ZoneId.of("Europe/Helsinki")) // 2024-03-11T16:24:15.994825+02:00[Europe/Helsinki]
	
	// ZonedDateTime
	val nowAtUTC = ZonedDateTime.now(ZoneId.of("UTC")) // 2024-03-11T07:27:38.147302Z[UTC]
}
```

## Formatting: Date to String
```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
	val dateTime = LocalDateTime.now().format(DateTimeFormatter.ISO_DATE_TIME) // 2024-03-11T16:32:56.075643 // ISO_DATE_TIME과 동일
	
	val zonedDateTime = ZonedDateTime.now(ZoneId.of("UTC")).format(DateTimeFormatter.ISO_ZONED_DATE_TIME) // 2024-03-11T07:32:56.075693Z[UTC] // ISO_DATE_TIME과 동일
	val localZonedDateTime = ZonedDateTime.now(ZoneId.of("UTC")).format(DateTimeFormatter.ISO_LOCAL_DATE_TIME) // 2024-03-11T07:32:56.075693
	
	val pattern = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")) // 2024-03-11 16:37:57
	
	val localizedLongStyle = ZonedDateTime.now(ZoneId.of("UTC")).format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG)) // 2024년 3월 11일 화요일 오전 7시 42분 41초 UTC
	val franceLocalizedMediumLongStyle = ZonedDateTime.now(ZoneId.of("UTC")).format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM, FormatStyle.LONG).withLocale(Locale.FRENCH)) // 11 mars 2024, 07:42:41 UTC
} else {
	val localizedDateTime = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT).format(Date())  // 2024. 3. 11. 오후 9:13
	val ukLocalizedDateTime = DateFormat.getDateTimeInstance(DateFormat.MEDIUM, DateFormat.SHORT, Locale.UK).format(Date())
}
```


## Extensions
```kotlin
fun String.toLocalDateString(): String {
	val simpleDateFormat = SimpleDateFormat("yyyy-MM-dd HH:mm:ss") // 2024-03-12 08:00:00
	val date = simpleDateFormat.parse(this)  
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT)  
	return dateFormat.format(date) // Mar 12, 2024 8:00 AM
}

fun Date.toLocalDateString(): String { // Tue Mar 12 08:00:00 GMT+09:00 2024
	val dateFormat = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.SHORT)  
	return dateFormat.format(this) // Mar 12, 2024 8:00 AM
}
```

https://madplay.github.io/post/reasons-why-javas-date-and-calendar-was-bad