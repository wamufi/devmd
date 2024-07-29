---
title: Modifying state during view update, this will cause undefined behavior.
date: 2024-07-29
time: 03:45:17
tags:
- ios
- swift
- swiftui
- error
---
뷰가 그려지는 동안 상태를 업데이트해서 발생

샘플 코드는 대충 만든 것
`DispatchQueue.main.async`에 상태 업데이트 코드를 넣어주면 된다.

```swift
struct TestView: View {

    @State private var text = "Hello"

    var body: some View {
        TextField("title", text: $text)
            .task {
                await updateText()
            }
    }

    func updateText() async {
        do {
            DispatchQueue.main.async {
                text = "World"
            }
        } catch {
            
        }
    }
}
```

https://www.hackingwithswift.com/quick-start/swiftui/how-to-fix-modifying-state-during-view-update-this-will-cause-undefined-behavior

https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E6%95%99%E5%AE%A4/swiftui-speedtracker-resolving-swiftui-modifying-state-during-view-update-issue-with-mapkit-1b75c58843ef
