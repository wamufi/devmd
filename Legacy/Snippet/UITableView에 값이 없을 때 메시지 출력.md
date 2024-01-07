---
boostnote:
  createdAt: '2022-01-11T08:08:34.827Z'
  updatedAt: '2022-09-30T01:33:37.040Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView에 값이 없을 때 메시지 출력
  tags:
    - Swift
    - iOS
    - UITableView
  linesHighlighted: []
  isStarred: false
  key: 6e79a2cb-3a53-4e6e-8205-05a8cef53c23
  storage: d112f8ec1e85e056a09d
---

UITableView에 값이 없을 때 메시지 출력
---
```swift
extension UITableView {
    /// 테이블 뷰가 비었을 때 출력할 메시지
    func setEmptyMessage(_ message: String) {
        let messageLabel = UILabel(frame: CGRect(x: 0, y: 0, width: bounds.size.width, height: bounds.size.height))
        messageLabel.text = message
        messageLabel.textColor = .secondaryLabel
        messageLabel.numberOfLines = 0
        messageLabel.textAlignment = .center
        messageLabel.font = .boldSystemFont(ofSize: 15)
        messageLabel.sizeToFit()

        backgroundView = messageLabel
        separatorStyle = .none
    }

    /// setEmptyMessage 사용 후 원래대로 되돌릴 때 (테이블 뷰에 값이 있을 때)
    func restore() {
        backgroundView = nil
        separatorStyle = .singleLine
    }
}
```

사용은
```swift
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    let count = hmDBViewModel.getInfoCount()
    if count <= 0 {
        tableView.setEmptyMessage("로또 번호를 추가해주세요.")
    } else {
        tableView.restore()
    }

    return count
}
```

https://stackoverflow.com/questions/15746745/handling-an-empty-uitableview-print-a-friendly-message

+) IntrinsicTableView와 동시 사용 불가! 쓰고 싶으면 cell로 하거나
```swift
private lazy var emptyCollectionViewHeight = { [weak self] in
    self?.collectionView.heightAnchor.constraint(equalToConstant: 300)
}()

func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    if productCount <= 0 {
        collectionView.setEmptyMessage("상품을 준비중입니다.\n빠른 시일 내에 좋은 상품으로 찾아뵙겠습니다.")
        emptyCollectionViewHeight?.isActive = true
    } else {
        collectionView.restore()
        emptyCollectionViewHeight?.isActive = false
    }

    return productCount
}
```
처리 필요

근데 셀보다는 이게 편해보임 아마도