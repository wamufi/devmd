---
boostnote:
  createdAt: '2022-09-13T08:21:40.746Z'
  updatedAt: '2022-09-13T08:22:25.001Z'
  type: MARKDOWN_NOTE
  folder: f9b4c2c416b7c9c01097
  title: UITableView Lifecycle
  tags: []
  linesHighlighted: []
  isStarred: false
  key: e96881a1-b61a-4186-8d92-0ce83a238e58
  storage: d112f8ec1e85e056a09d
---

UITableView Lifecycle
---
didSelect -> estimatedHeight -> height

tableView.reloadData() 할 경우 headerView의 awakeFromNib() 호출됨
당연한가..?