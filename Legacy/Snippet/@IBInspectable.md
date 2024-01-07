---
boostnote:
  createdAt: '2020-08-06T00:44:42.840Z'
  updatedAt: '2020-08-06T00:45:40.170Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: '@IBInspectable'
  tags:
    - Swift
    - UI
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 563d9412-bc24-49c3-8453-f2ef5af740db
  storage: d112f8ec1e85e056a09d
---

@IBInspectable
---
storyboard에서 bool 값을 받아 다르게 적용
```swift
@IBInspectable var hasNotch: Bool = false {
    didSet {
        if hasNotch {
            addHeightConstant()
        }
    }
}
```