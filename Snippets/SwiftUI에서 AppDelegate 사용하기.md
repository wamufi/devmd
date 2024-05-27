---
title: SwiftUI에서 AppDelegate 사용하기
date: 2024-05-26
time: 22:43:13
tags:
- swift
- swiftui
- ios
---
AsdfApp.swift
```swift
import SwiftUI

@main
struct AsdfApp: App {
    @NSApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

class AppDelegate: NSObject, NSApplicationDelegate {

    func applicationDidFinishLaunching(_ notification: Notification) {
        print("finishLaunching: \(notification)")
    }
}

```