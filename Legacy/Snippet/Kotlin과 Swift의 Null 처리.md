---
boostnote:
  createdAt: '2021-04-14T02:02:53.398Z'
  updatedAt: '2021-04-14T08:57:59.187Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Kotlin과 Swift의 Null 처리
  tags:
    - Kotlin
    - Swift
  linesHighlighted: []
  isStarred: false
  key: 9c8b8a94-8ce5-4594-a845-c0bb9bcd578e
  storage: d112f8ec1e85e056a09d
---

Kotlin과 Swift의 Null 처리
---
Swift에서 if let과 guard let 정말 기가 막히게 잘 써먹었다.
그래서 Kotlin에서도 써먹으려고 정리

```kotlin
b?.let {
    println("b is not null $it")
} ?: run {
    println("b is null")
}
```

```swift
if let a = b {
    print("b is not null \(a)")
} else {
    print("b is null")
}

guard let a = b else {
    print("b is null")
}
```

[Swift 'if let' statement equivalent in Kotlin - Stack Overflow](https://stackoverflow.com/questions/46723729/swift-if-let-statement-equivalent-in-kotlin)
[Kotlin 안전한 null 처리](https://thdev.tech/kotlin/2016/08/04/Kotlin-Null-Safety)
[투덜이의 리얼 블로그 :: [Kotlin] 코틀린 null 처리 - ? ?. ?: !!, let, lateinit, 제너릭, 플랫폼 타입](https://tourspace.tistory.com/114)
