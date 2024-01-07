---
boostnote:
  createdAt: '2019-12-30T01:53:19.661Z'
  updatedAt: '2019-12-30T01:55:20.263Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableViewCell 높이 동적
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 40d4bcbf-4e71-4149-8ff4-7cd6bd5d8f31
  storage: d112f8ec1e85e056a09d
---

UITableViewCell 높이 동적
---
UITableViewCell dynamic height

```objc
- (void)viewDidLoad {
    _tableView.estimatedRowHeight = 44.0;
    _tableView.rowHeight = UITableViewAutomaticDimension;
}

- (CGFloat)tableView:(UITableView *)tableView 
heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return UITableViewAutomaticDimension;
}
```