---
boostnote:
  createdAt: '2022-10-06T07:10:57.277Z'
  updatedAt: '2022-10-06T07:11:46.885Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UICollectionView Dynamic Height
  tags:
    - Swift
    - iOS
    - UICollectionView
  linesHighlighted: []
  isStarred: false
  key: 61a94cc8-20f0-40d0-88d7-30baeb3b088b
  storage: d112f8ec1e85e056a09d
---

UICollectionView Dynamic Height
---
```swift
class DynamicHeightCollectionView: UICollectionView {
    override func layoutSubviews() {
        super.layoutSubviews()
        if !__CGSizeEqualToSize(bounds.size, self.intrinsicContentSize) {
            self.invalidateIntrinsicContentSize()
        }
    }
    
    override var intrinsicContentSize: CGSize {
        return contentSize
    }
}
```

https://velog.io/@gaebokchi/IntrinsicContentSize를-이용한-Dynamic-Height

IntrinsicCollectionView 안될 때 쓰면 작동 하는 듯