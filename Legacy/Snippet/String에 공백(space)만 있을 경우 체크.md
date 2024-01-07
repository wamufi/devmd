---
boostnote:
  createdAt: '2022-10-05T07:42:22.790Z'
  updatedAt: '2022-10-05T07:43:23.629Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: String에 공백(space)만 있을 경우 체크
  tags:
    - Swift
    - iOS
    - String
  linesHighlighted: []
  isStarred: false
  key: 7fc01334-266a-4b25-a263-7d2f2d702541
  storage: d112f8ec1e85e056a09d
---

String에 공백(space)만 있을 경우 체크
---
```swift
if text.trimmingCharacters(in: .whitespaces).isEmpty {
    // 공백만 있음
} else {
    // 문자 존재
}
```

https://stackoverflow.com/questions/27768064/check-if-swift-text-field-contains-non-whitespace