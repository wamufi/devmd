---
boostnote:
  createdAt: '2021-12-22T06:37:42.209Z'
  updatedAt: '2021-12-22T07:31:20.334Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UICollectionView에서 줄 당 셀 숫자 지정
  tags:
    - UICollectionView
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: dafc9f6e-0140-466c-b191-7556f1b4055e
  storage: d112f8ec1e85e056a09d
---

UICollectionView에서 줄 당 셀 숫자 지정
---
여기서는 한 줄 당 7개의 셀을 지정
특이사항 insets, spacing이 10이어야 작동함... 왜일까

[ios - UICollectionView Set number of columns - Stack Overflow](https://stackoverflow.com/questions/14674986/uicollectionview-set-number-of-columns)
1번 답의 #3 Using `UICollectionViewDelegateFlowLayout's collectionView(_:layout:sizeForItemAt:)` method 사용


```swift
extension MyLottoNumPopUpViewController: UICollectionViewDelegateFlowLayout {
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        let inset: CGFloat = 10
        let minimumInteritemSpacing: CGFloat = 10
        let cellsPerRow = 7
        
        let marginsAndInsets = inset * 2 + collectionView.safeAreaInsets.left + collectionView.safeAreaInsets.right + minimumInteritemSpacing * CGFloat(cellsPerRow - 1)
        let itemWidth = ((collectionView.bounds.size.width - marginsAndInsets) / CGFloat(cellsPerRow)).rounded(.down)
        return CGSize(width: itemWidth, height: itemWidth * 1.3)
    }
    
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets {
        return UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
    }
    
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumLineSpacingForSectionAt section: Int) -> CGFloat {
        return 10
    }
    
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAt section: Int) -> CGFloat {
        return 10
    }
}
```