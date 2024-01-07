---
boostnote:
  createdAt: '2022-01-27T08:15:51.444Z'
  updatedAt: '2022-01-27T08:21:39.701Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'Swift: didSet & Kotlin: Delegates.Observable'
  tags:
    - iOS
    - Swift
    - Android
    - Kotlin
  linesHighlighted: []
  isStarred: false
  key: bc92ad75-65c6-437c-87be-63d21b327708
  storage: d112f8ec1e85e056a09d
---

Swift: didSet & Kotlin: Delegates.Observable
---
```swift
var numberString: String = "0" {
    didSet {
        numberLabel.text = numberString
        setupBackgroundColor()
    }
}
```

```kotlin
var numberString: String by Delegates.observable("0") { property, oldValue, newValue ->
    Log.v("CircleView", "$property is $newValue from $oldValue")
    binding.circleText.text = numberString
    binding.circleView.background = drawCircle()
}
```

https://leveloper.tistory.com/169
