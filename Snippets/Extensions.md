---
title: Extensions
date: 2024-09-07
time: 04:32:48
tags:
- swift
---
```swift
extension Optional where Wrapped: Collection {

	var isNullOrEmpty: Bool { return self?.isEmpty ?? true }
    var isNotNullOrEmpty: Bool { return self?.isEmpty == false }
}

extension String {

	var trimmedLeadingWhitespace: String { return self.trimmingCharacters(in: .whitespaces) } // 맨 앞의 공백을 제거
}
```