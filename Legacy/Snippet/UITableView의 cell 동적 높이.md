---
boostnote:
  createdAt: '2020-03-20T06:36:35.735Z'
  updatedAt: '2020-03-20T06:38:38.440Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView의 cell 동적 높이
  tags:
    - Objective-C
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: d3ccb7e8-db65-4c8d-80b3-e715a8c72a87
  storage: d112f8ec1e85e056a09d
---

UITableView의 cell 동적 높이
---
```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return UITableViewAutomaticDimension;
}
```

```swift
func tableView(_ tableView: UITableView, estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
    return UITableView.automaticDimension
}
```

Top, Bottom에 Constraints가 지정되어 있어야 한다.


[ios - Dynamic Height Issue for UITableView Cells (Swift) - Stack Overflow](https://stackoverflow.com/questions/30494702/dynamic-height-issue-for-uitableview-cells-swift)