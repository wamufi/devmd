---
boostnote:
  createdAt: '2020-08-06T00:46:02.003Z'
  updatedAt: '2020-08-06T00:46:49.102Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: storyboard에서 설정한 constraint를 가져와서 constant 변경
  tags:
    - Swift
    - UI
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 7f00cc9e-5ce0-456f-98e5-c592eef8097e
  storage: d112f8ec1e85e056a09d
---

storyboard에서 설정한 constraint를 가져와서 constant 변경
---
```swift
for constraint in self.constraints {
    if constraint.firstAttribute == .height { // 높이
        constraint.constant += 10
    }
}
```