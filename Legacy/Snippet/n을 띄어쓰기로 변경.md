---
boostnote:
  createdAt: '2020-12-02T08:23:29.648Z'
  updatedAt: '2020-12-02T08:23:55.544Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: \n을 띄어쓰기로 변경
  tags:
    - String
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: ca1c40bb-8adb-4c46-972a-339ad7e84252
  storage: d112f8ec1e85e056a09d
---

\n을 띄어쓰기로 변경
---
```swift
var giftItem: String = viewModel.giftHistoryGiftItmViewItemAtIndexPath(indexPath)
while let rangeToReplace = giftItem.range(of: "\n") {
    giftItem.replaceSubrange(rangeToReplace, with: " ")
}
cell.giftItmLabel.text = giftItem
```