---
title: Json Encoding & Decoding
date: 2024-02-22
time: 02:10:48
tags:
  - android
  - kotlin
  - ios
  - swift
  - json
---
디코딩: json(string)을 객체로
인코딩: 객체를 json(string)으로

kotlinx
```
val json = Json { ignoreUnknownKeys = true }

val decoding = json.decodeFromString<Args>(jsonString)
val encoding = json.encodeToString(args)
```

swift
```
if let jsonData = jsonString.data(using: .utf8) {
	do {
		let decoding = try JSONDecoder().decode(Args.self, from: jsonData)
	} catch {
		print("error")
	}
}


if let jsonData = try? JSONEncoder().encode(args) {
	let encoding = String(data: jsonData, encoding: .utf8)!
}
```

자꾸 까먹어서...  moshi, gson 추가 예정