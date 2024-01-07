---
boostnote:
  createdAt: '2019-12-30T01:55:21.397Z'
  updatedAt: '2020-01-02T06:48:15.901Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UILabel 라인 제한 없이 표시
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 703927f4-0915-422a-b706-f4df64d9bc9f
  storage: d112f8ec1e85e056a09d
---

UILabel 라인 제한 없이 표시
---
UILabel multiple lines

```objc
- (void)setupLabel {
    _label.numberOfLines = 0;
    _label.lineBreakMode = NSLineBreakByWordWrapping;
}
```