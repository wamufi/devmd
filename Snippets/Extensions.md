---
title: Extensions
date: 2024-09-07
time: 04:32:48
tags:
- swift
---
```swift
extension Optional where Wrapped: Collection {
    var isNotNullOrEmpty: Bool {
        return self?.isEmpty == false
    }
}
```