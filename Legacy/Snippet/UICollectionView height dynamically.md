---
boostnote:
  createdAt: '2020-09-16T01:41:22.572Z'
  updatedAt: '2022-07-14T05:35:07.295Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UICollectionView height dynamically
  tags:
    - Swift
    - UICollectionView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: de65429c-df44-477c-8d49-8a2ee286cf1c
  storage: d112f8ec1e85e056a09d
---

UICollectionView height dynamically
---
높이를 자동으로 바꿔보자

collection view는 높이 조절이 자동으로 안 된다.
일단 height constraint를 지정해주고 시작하자.
그냥 heightAnchor로 하면 충돌난다. 원래 0이었던거랑 내가 지정한거랑.

```swift
override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()
    collectionViewHeight.constant = collectionView.collectionViewLayout.collectionViewContentSize.height
}
```


이게 더 편한가...?
```swift
import UIKit

class IntrinsicCollectionView: UICollectionView {

    override func layoutSubviews() {
        super.layoutSubviews()
        if bounds.size != intrinsicContentSize {
            invalidateIntrinsicContentSize()
        }
    }
    
    override var intrinsicContentSize: CGSize {
        self.contentSize
    }
}
```

```swift
giveAwayCollectionViewHeight.constant = giveAwayCollectionView.intrinsicContentSize.height
```