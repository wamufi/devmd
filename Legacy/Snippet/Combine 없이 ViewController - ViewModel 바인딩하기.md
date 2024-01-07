---
boostnote:
  createdAt: '2022-11-17T06:08:44.741Z'
  updatedAt: '2022-11-17T06:10:58.003Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Combine 없이 ViewController - ViewModel 바인딩하기
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 40d25d08-5adf-4b0b-a4d0-14e7897adb15
  storage: d112f8ec1e85e056a09d
---

Combine 없이 ViewController - ViewModel 바인딩하기
---
ViewModel.swift
```swift
class ViewModel: NSObject {
    var documents: [MapKitStruct]? {
        didSet {
            self.bindViewModel()
        }
    }
    
    var bindViewModel: (() -> Void) = { }
}
```

ViewController.swift
```swift
override func viewWillAppear(_ animated: Bool) {
    viewModel.bindViewModel = {
        ...
    }
}
```