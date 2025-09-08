---
title: 버전에 따른 분기처리
date:  2025-09-08
time:  10:09:12
tags:
  - swift
  - ios
---
```swift
extension View {
    func modify<T: View>(@ViewBuilder _ modifier: (Self) -> T) -> some View {

        return modifier(self)

    }
}
```

```swift
Button {
    let _ = print("!!!")
} label: {
    Image(systemImage: "xmark")
}
.modify {
    if #available(iOS 26.0, *) {
        $0.buttonStyle(.glass)
    } else {
        $0.buttonStyle(.plain)
    }
}
```