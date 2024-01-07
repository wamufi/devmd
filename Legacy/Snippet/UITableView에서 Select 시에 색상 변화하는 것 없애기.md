---
boostnote:
  createdAt: '2018-01-25T04:33:33.190Z'
  updatedAt: '2019-03-06T03:09:12.155Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView에서 Select 시에 색상 변화하는 것 없애기
  tags:
    - Objective-C
    - UITableView
    - iOS
  isStarred: false
  linesHighlighted: []
  key: c8a084f4557c29adeaf4
  storage: d112f8ec1e85e056a09d
---

UITableView에서 Select 시에 색상 변화하는 것 없애기
---
```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    [tableView deselectRowAtIndexPath:indexPath animated:true];
    
    // 코드 작성
}
```