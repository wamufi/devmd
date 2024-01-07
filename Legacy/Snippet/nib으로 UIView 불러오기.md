---
boostnote:
  createdAt: '2022-07-14T01:47:16.951Z'
  updatedAt: '2022-07-14T01:48:25.415Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: nib으로 UIView 불러오기
  tags:
    - Swift
    - iOS
    - UIView
    - xib
  linesHighlighted: []
  isStarred: false
  key: 017a30ca-4ac8-4973-a19c-2db683373821
  storage: d112f8ec1e85e056a09d
---

nib으로 UIView 불러오기
---
```swift
extension UIView {
    class func fromNib<T: UIView>() -> T {
        return Bundle(for: T.self).loadNibNamed(String(describing: T.self), owner: nil, options: nil)![0] as! T
    }
}
```

```swift
let myCustomView: CustomView = UIView.fromNib()
let myCustomView: CustomView = .fromNib()
```

https://stackoverflow.com/questions/24857986/load-a-uiview-from-nib-in-swift
