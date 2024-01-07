---
boostnote:
  createdAt: '2021-12-23T06:49:36.332Z'
  updatedAt: '2021-12-23T07:53:54.881Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UICollectionView Header height dynamically
  tags:
    - iOS
    - Swift
    - UICollectionView
  linesHighlighted: []
  isStarred: false
  key: 6e291a2b-6552-4695-9d5a-c63adc18fe2e
  storage: d112f8ec1e85e056a09d
---

UICollectionView Header height dynamically
---
ReusableView 얘기하는 것 맞다
```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize {
    if let headerView = collectionView.supplementaryView(forElementKind: UICollectionView.elementKindSectionHeader, at: IndexPath(item: 0, section: 0)) as? MyLottoNumCollectionReusableView {
        return headerView.systemLayoutSizeFitting(CGSize(width: collectionView.frame.width, height: UIView.layoutFittingExpandedSize.height), withHorizontalFittingPriority: .required, verticalFittingPriority: .fittingSizeLevel)
    } else {
        let headerView = self.collectionView(collectionView, viewForSupplementaryElementOfKind: UICollectionView.elementKindSectionHeader, at: IndexPath(item: 0, section: 0))

        return headerView.systemLayoutSizeFitting(CGSize(width: collectionView.frame.width, height: UIView.layoutFittingExpandedSize.height), withHorizontalFittingPriority: .required, verticalFittingPriority: .fittingSizeLevel)
    }
}
```

https://stackoverflow.com/questions/39825290/uicollectionview-header-dynamic-height-using-auto-layout