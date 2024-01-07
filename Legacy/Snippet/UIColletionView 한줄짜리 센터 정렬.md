---
boostnote:
  createdAt: '2020-10-21T05:02:17.201Z'
  updatedAt: '2021-12-16T06:53:47.221Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIColletionView 한줄짜리 센터 정렬
  tags:
    - Swift
    - UICollectionView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 5df11405-2f14-406c-aa4a-720ca20a8f3f
  storage: d112f8ec1e85e056a09d
---

UIColletionView 한줄짜리 센터 정렬
---
두 줄 이상 넘어가는 건 센터 정렬 ㄴㄴ 꽉차게 해서 그냥 5/5/2 이런 느낌으로 떨어짐
```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets {
    if collectionView == missionCollecitonView {
        let cellCount = missionCollecitonView.numberOfItems(inSection: section)
        let cellWidth: CGFloat = 50
        
        // cellCount가 1일 때는 안 나오더라. 이유는 안 찾아봤다.
        if cellCount < 2 {
            cellWidth += 50
        }

        let totalCellWidth = Int(cellWidth) * cellCount
        let totalSpacingWidth = 15 * (cellCount - 1)

        // horizontalInset이 -일 경우 collectionView.frame.width 초과해서 나온다.
        if horizontalInset < 0 {
            horizontalInset = 0
        }

        return UIEdgeInsets(top: 0, left: horizontalInset, bottom: 0, right: horizontalInset)
    } else {
        return UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
    }
}
```

실제 사용한 코드
```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets {
    if collectionView == wordCollectionView {
        let cellCount = wordCollectionView.numberOfItems(inSection: section)
        var cellWidth: CGFloat = 40

        if cellCount < 2 {
            cellWidth += 40
        }

        let totalCellWidth = Int(cellWidth) * cellCount
        let totalSpacingWidth = 10 * (cellCount - 1)

        let numberOfCells = floor(collectionView.frame.size.width / cellWidth)
        let edgeInsets = (collectionView.frame.size.width - (numberOfCells * cellWidth)) / (numberOfCells + 1)

        var horizontalInset = (collectionView.frame.width - CGFloat(totalCellWidth + totalSpacingWidth)) / 2

        if horizontalInset < 0 {
            horizontalInset = 0
        }

        return UIEdgeInsets(top: 0, left: horizontalInset, bottom: 0, right: horizontalInset)
    } else {
        return UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
    }
}
```

아래 코드는 사용 안한 듯?
```swift
class LastRowCenteredLayout: UICollectionViewFlowLayout {
    override func layoutAttributesForElements(in rect: CGRect) -> [UICollectionViewLayoutAttributes]? {
        let layoutAttributesObjects = super.layoutAttributesForElements(in: rect)?.map{ $0.copy() } as? [UICollectionViewLayoutAttributes]
        layoutAttributesObjects?.forEach({ layoutAttributes in
            if layoutAttributes.representedElementCategory == .cell {
                if let newFrame = layoutAttributesForItem(at: layoutAttributes.indexPath)?.frame {
                    layoutAttributes.frame = newFrame
                }
            }
        })
        return layoutAttributesObjects
    }

    override func layoutAttributesForItem(at indexPath: IndexPath) -> UICollectionViewLayoutAttributes? {
        guard let collectionView = collectionView else {
            fatalError()
        }
        guard let layoutAttributes = super.layoutAttributesForItem(at: indexPath)?.copy() as? UICollectionViewLayoutAttributes else {
            return nil
        }

        layoutAttributes.frame.origin.x = sectionInset.left
        layoutAttributes.frame.size.width = collectionView.safeAreaLayoutGuide.layoutFrame.width - sectionInset.left - sectionInset.right
        
        return layoutAttributes
    }
}
```