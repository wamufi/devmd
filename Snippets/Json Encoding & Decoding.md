---
title: Json Encoding & Decoding
date: 2024-02-22
time: 02:10:48
tags:
  - android
  - kotlin
  - ios
  - swift
---
kotlinx
```
val json = Json { ignoreUnknownKeys = true }

val decoding = json.decodeFromString<Args>(jsonString)
val encoding = json.encodeToString(args)
```

swift
```1
let decoding = try? JSONDecoder().decode(Args.self, from: jsonString)
let encoding = try? JSONEncoder().encode(args)
```