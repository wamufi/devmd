---
boostnote:
  createdAt: '2021-12-20T05:46:53.711Z'
  updatedAt: '2021-12-22T07:54:59.064Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UICollectionViewCell에서 UIButton 이벤트 처리
  tags:
    - UIButton
    - UICollectionView
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 247e6bea-7ddf-4279-81a7-13253499035e
  storage: d112f8ec1e85e056a09d
---

UICollectionViewCell에서 UIButton 이벤트 처리
---
ViewController
```swift
// CollectionView 터치 처리
@objc func handleCheckButtonTapGesture(_ sender: CheckButton) {
    if sender.isSelected {
        sender.isSelected = false
    } else {
        sender.isSelected = true
    }
}
```

Extension CollectionViewDelegate 처리
```swift
extension MyLottoNumPopUpViewController: UICollectionViewDelegate, UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "MyLottoNumCollectionViewCell", for: indexPath) as! MyLottoNumCollectionViewCell
        cell.checkButton.addTarget(self, action: #selector(handleCheckButtonTapGesture(_:)), for: .touchUpInside)
        
        return cell
    }
}
```

또는
```swift
extension MyLottoNumPopUpViewController: UICollectionViewDelegate, UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "MyLottoNumCollectionViewCell", for: indexPath) as! MyLottoNumCollectionViewCell
        cell.contentView.isUserInteractionEnabled = false // 꼭 해줘야 함 cell은 user interaction enabled true
        cell.checkButton.setTitle("\(indexPath.row + 1)", for: .normal)
//        cell.checkButton.addTarget(self, action: #selector(handleCheckButtonTapGesture(_:)), for: .touchUpInside)
        
        return cell
    }
    
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        let cell = collectionView.cellForItem(at: indexPath)
        guard let typedCell = cell as? MyLottoNumCollectionViewCell else { return }
        typedCell.checkButton.isSelected.toggle()
        
        if typedCell.checkButton.isSelected {
            print("typedCell.checkButton.currentTitle: \(typedCell.checkButton.currentTitle)")
        }
    }
}
```
그리고 