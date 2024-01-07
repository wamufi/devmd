---
boostnote:
  createdAt: '2022-10-06T02:12:58.783Z'
  updatedAt: '2022-10-06T02:13:49.990Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIButton.edgeInsets 안 먹을 경우
  tags:
    - Swift
    - iOS
    - UIButton
  linesHighlighted: []
  isStarred: false
  key: 7e863e67-20e7-4b98-93e5-05dcdd8345aa
  storage: d112f8ec1e85e056a09d
---

UIButton.edgeInsets 안 먹을 경우
---
```swift
button.contentEdgeInsets = UIEdgeInsets(top: 0.01, left: 0, bottom: 0.01, right: 5)
```

0 대신에 0.01을 넣자 ㅡㅡ