---
boostnote:
  createdAt: '2020-11-27T05:37:54.996Z'
  updatedAt: '2020-11-27T05:39:32.912Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: YouTube ID 가져오기
  tags:
    - iOS
    - swift
  linesHighlighted: []
  isStarred: false
  key: 507b1d53-7eb7-4e19-ba02-71f459560252
  storage: d112f8ec1e85e056a09d
---

YouTube ID 가져오기
---
```swift
var youtubeID: String {
    let pattern = "((?<=(v|V)/)|(?<=be/)|(?<=(\\?|\\&)v=)|(?<=embed/))([\\w-]++)"

    let regex = try? NSRegularExpression(pattern: pattern, options: .caseInsensitive)
    let range = NSRange(location: 0, length: count)

    guard let result = regex?.firstMatch(in: self, range: range) else {
        return self
    }

    return (self as NSString).substring(with: result.range)
}
```

뒤에 동영상 id로 주로 돌리는 것 같아 가져와서 처리.