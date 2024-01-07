---
boostnote:
  createdAt: '2022-07-05T07:38:03.633Z'
  updatedAt: '2022-07-05T07:39:48.016Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UILabel 왼쪽에 이미지 추가
  tags: []
  linesHighlighted: []
  isStarred: false
  key: c805e941-537a-4848-be83-c245bbca73a3
  storage: d112f8ec1e85e056a09d
---

UILabel 왼쪽에 이미지 추가
---
```swift
extension NSMutableAttributedString {
    /// 텍스트 왼쪽에 이미지 추가
    func addLeadingImage(_ image: UIImage, string: String) -> NSMutableAttributedString {
        // 이미지
        let imageAttachment = NSTextAttachment()
        imageAttachment.image = image
        let imageOffsetY: CGFloat = -5.0
        imageAttachment.bounds = CGRect(x: 0, y: imageOffsetY, width: imageAttachment.image!.size.width, height: imageAttachment.image!.size.height)
        
        // 패딩
        let padding = NSTextAttachment()
        padding.bounds = CGRect(x: 5, y: 0, width: 5, height: 5)
        
        let attributedString = NSMutableAttributedString(string: "")
        attributedString.append(NSAttributedString(attachment: imageAttachment))
        attributedString.append(NSAttributedString(attachment: padding))
        attributedString.append(NSAttributedString(string: string)) // 텍스트
        
        return attributedString
    }
}
```

```swift
addressLabel.attributedText = NSMutableAttributedString().addLeadingImage(UIImage(systemName: "mappin.and.ellipse")!, string: place.addressName)
```