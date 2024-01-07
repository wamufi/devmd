---
boostnote:
  createdAt: '2022-01-05T07:30:06.969Z'
  updatedAt: '2022-01-05T07:48:42.075Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 정규식을 이용한 String parsing
  tags:
    - Regex
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: f1f9ca40-87e0-491c-b656-7a1c6fa9bce2
  storage: d112f8ec1e85e056a09d
---

정규식을 이용한 String parsing
---
```swift
private func parseUrl(_ url: String) {
    var timesString = ""

    let timesRegex = "(?<=\\?v=).{0,4}"
    let timesExpression = try? NSRegularExpression(pattern: timesRegex, options: .caseInsensitive)
    let timesRange = NSRange(location: 0, length: url.utf16.count)
    if let timesMatch = timesExpression?.firstMatch(in: url, options: [], range: timesRange) {
        // substring 사용 시
//        timesString = url.substring(with: timesMatch.range) ?? ""

        if let range = Range(timesMatch.range, in: url) {
            print("\(url[range])")
        }
    }

    var lottos: [String] = []

    let lottoRegex = "(?<=m).{0,12}(?=m)"
    let lottoExpression = try? NSRegularExpression(pattern: lottoRegex, options: .caseInsensitive)
    let lottoRange = NSRange(location: 0, length: url.utf16.count)
    lottoExpression?.enumerateMatches(in: url, options: [], range: lottoRange, using: { (result, flag, stop) in
        guard let result = result else { return }
        if let range = Range(result.range, in: url) {
            lottos.append("\(url[range])")
        }
    })

    print("lottos: \(lottos)")
}
```

[String substring with NSRange](:note:69377d1e-59a5-4dc8-ac2f-0e796ce25734)

위의 substring과 같이 이용하면 더 편할 수도
`// timesString = url.substring(with: timesMatch.range) ?? ""`
이거는 풀어줘야함