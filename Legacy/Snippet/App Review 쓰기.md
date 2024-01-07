App Review 쓰기
---
SwiftUI
```swift
Link("Write a Review", destination: URL(string: "https://apps.apple.com/app/id6458879880?action=write-review")!)
```

Swift
```swift
// 리뷰 작성 화면으로 이동
private func writeAppStoreReview() {
    guard let writeReviewURL = URL(string: "https://apps.apple.com/app/id6458879880?action=write-review") else { fatalError("Expected a valid URL") }
    UIApplication.shared.open(writeReviewURL, options: [:], completionHandler: nil)
}
```