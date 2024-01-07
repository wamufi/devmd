---
boostnote:
  createdAt: '2020-08-21T01:14:24.646Z'
  updatedAt: '2022-08-31T04:49:35.480Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'UITableView, UICollectionView inside UIScrollView'
  tags:
    - Swift
    - UIScrollView
    - UITableView
    - iOS
    - UICollectionView
  linesHighlighted: []
  isStarred: false
  key: cc4dd73a-b1f0-4071-9846-7cb358fa25e0
  storage: d112f8ec1e85e056a09d
---

UITableView, UICollectionView inside UIScrollView
---
```swift
/// 스크롤 뷰 안에 테이블 뷰 만들 때 사용
class IntrinsicTableView: UITableView {

    override var contentSize: CGSize {
        didSet {
            self.invalidateIntrinsicContentSize()
        }
    }

    override var intrinsicContentSize: CGSize {
        self.layoutIfNeeded()
        return CGSize(width: UIView.noIntrinsicMetric, height: contentSize.height)
    }

}

/// 스크롤 뷰 안에 컬렉션 뷰 사용할 때
class IntrinsicCollectionView: UICollectionView {

    override var contentSize: CGSize {
        didSet {
            self.invalidateIntrinsicContentSize()
        }
    }

    override var intrinsicContentSize: CGSize {
        self.layoutIfNeeded()
        return CGSize(width: UIView.noIntrinsicMetric, height: contentSize.height)
    }

}
```

근데 이거 꼭 써야하는지 모르겠당,,,
+) 안 쓰면 사이즈 안 늘어남 ㅋ