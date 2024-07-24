---
title: UIKit에 SwiftUI 추가하기
date: 2024-07-24
time: 04:34:51
tags:
- ios
- swift
- swiftui
---
**ShopHosintController.swift**
```swift
import UIKit
import SwiftUI

class ShopHostingController: UIHostingController<ShopView> {
    required init?(coder aDecoder: NSCoder) {
         super.init(coder: aDecoder, rootView: ShopView())
     }
}

```

**Main.storyboard**
HostingViewController 추가 -> Custom Class를 `ShopHostingController`로 지정
