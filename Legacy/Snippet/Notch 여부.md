---
boostnote:
  createdAt: '2020-08-06T01:13:15.763Z'
  updatedAt: '2020-08-06T01:13:36.721Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Notch 여부
  tags:
    - Swift
    - UI
    - iOS
  linesHighlighted: []
  isStarred: false
  key: f0ffc5e4-d99f-4c07-8598-56c83e1de238
  storage: d112f8ec1e85e056a09d
---

Notch 여부
---
```swift
var hasBottomNotch: Bool {
    return UIApplication.shared.keyWindow?.safeAreaInsets.bottom ?? 0 > 0
}
```