---
boostnote:
  createdAt: '2022-01-05T07:04:59.631Z'
  updatedAt: '2022-01-05T07:11:00.507Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: String substring with NSRange
  tags:
    - Swfit
    - iOS
    - String
    - Range
  linesHighlighted: []
  isStarred: false
  key: 69377d1e-59a5-4dc8-ac2f-0e796ce25734
  storage: d112f8ec1e85e056a09d
---

String substring with NSRange
---
```swift
extension String {
    func substring(with nsrange: NSRange) -> String? {
        guard let range = Range(nsrange, in: self) else { return nil }
        return String(self[range])
    }
}
```