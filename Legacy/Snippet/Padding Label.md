---
boostnote:
  createdAt: '2022-07-14T07:15:42.825Z'
  updatedAt: '2022-07-14T07:16:12.354Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Padding Label
  tags:
    - Swift
    - iOS
    - UILabel
  linesHighlighted: []
  isStarred: false
  key: 2a5f1770-596c-40dc-b1cb-4ad7465888c8
  storage: d112f8ec1e85e056a09d
---

Padding Label
---
마진, 패딩이 필요한 레이블을 생성
```swift
/// 레이블에 패딩이 필요할 때 사용
@IBDesignable class PaddingLabel: UILabel {

    @IBInspectable var topInset: CGFloat = 0
    @IBInspectable var bottomInset: CGFloat = 0
    @IBInspectable var leftInset: CGFloat = 0
    @IBInspectable var rightInset: CGFloat = 0
    
    override func drawText(in rect: CGRect) {
        let insets = UIEdgeInsets(top: topInset, left: leftInset, bottom: bottomInset, right: rightInset)
        super.drawText(in: rect.inset(by: insets))
    }
    
    override var intrinsicContentSize: CGSize {
        let size = super.intrinsicContentSize
        return CGSize(width: size.width + leftInset + rightInset, height: size.height + topInset + bottomInset)
    }
    
    override var bounds: CGRect {
        didSet {
            preferredMaxLayoutWidth = bounds.width - (leftInset + rightInset)
        }
    }
}

```