---
boostnote:
  createdAt: '2022-11-17T04:55:37.281Z'
  updatedAt: '2022-11-17T04:56:09.478Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: '`swift'
  tags:
    - iOS
    - Swift
    - Google
    - GoogleMaps
  linesHighlighted: []
  isStarred: false
  key: 87a2c200-b312-4b15-b5e4-78d40f36443a
  storage: d112f8ec1e85e056a09d
---

```swift
extension GMSMarker {
    /// 마커 아이콘 사이즈 변경 (배수)
    /// - parameters
    ///   - multiple: 1.5배일 경우 1.5, 1.5배 작은 수를 넣을 경우 2/3
    func setIconSize(multiple: CGFloat) {
        guard let icon = self.icon else { return }
        let newSize = CGSize(width: icon.size.width * multiple, height: icon.size.height * multiple)
        
        UIGraphicsBeginImageContextWithOptions(newSize, false, 0.0)
        icon.draw(in: CGRect(origin: .zero, size: newSize))
        let newImage = UIGraphicsGetImageFromCurrentImageContext()!
        UIGraphicsEndImageContext()
        
        self.icon = newImage
    }
}
```