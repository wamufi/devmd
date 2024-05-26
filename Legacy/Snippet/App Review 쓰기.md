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

#### AppStoreReviewManager 사용
AppStoreReviewManager.swift
```swift
import Foundation
import StoreKit

enum AppStoreReviewManager {
    static let minimumReviewWorthyActionCount = 5
    
    static func requestReviewIfAppropriate() {
        let defaults = UserDefaults.standard
        let bundle = Bundle.main
        
        var actionCount = defaults.integer(forKey: .reviewWorthyActionCount)
        actionCount += 1
        defaults.set(actionCount, forKey: .reviewWorthyActionCount)
        
        guard actionCount >= minimumReviewWorthyActionCount else { return }
        
        let bundleVersionKey = kCFBundleVersionKey as String
        let currentVersion = bundle.object(forInfoDictionaryKey: bundleVersionKey) as? String
        let lastVersion = defaults.string(forKey: .lastReviewRequestAppVersion)
        
        guard lastVersion == nil || lastVersion != currentVersion else { return }
        
        SKStoreReviewController.requestReview()
        
        defaults.set(0, forKey: .reviewWorthyActionCount)
        defaults.set(currentVersion, forKey: .lastReviewRequestAppVersion)
    }
}
```

UserDefaults+Key.swift
```swift
import Foundation

extension UserDefaults {
    enum Key: String {
        case reviewWorthyActionCount
        case lastReviewRequestAppVersion
    }
    
    func integer(forKey key: Key) -> Int {
        return integer(forKey: key.rawValue)
    }
    
    func string(forKey key: Key) -> String? {
        return string(forKey: key.rawValue)
    }
    
    func set(_ integer: Int, forKey key: Key) {
        set(integer, forKey: key.rawValue)
    }
    
    func set(_ object: Any?, forKey key: Key) {
        set(object, forKey: key.rawValue)
    }
}

```

```swift
AppStoreReviewManager.requestReviewIfAppropriate()
```

https://developer.apple.com/documentation/storekit/requesting_app_store_reviews
https://github.com/takecian/SwiftRater
