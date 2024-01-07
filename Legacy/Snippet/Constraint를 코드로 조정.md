---
boostnote:
  createdAt: '2019-11-26T06:20:40.964Z'
  updatedAt: '2019-11-26T06:36:28.258Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Constraint를 코드로 조정
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 1a08d054-7349-495a-9eef-4bc04752222d
  storage: d112f8ec1e85e056a09d
---

Constraint를 코드로 조정
---
```objc
imageView.translatesAutoresizingMaskIntoConstraints = false;

[imageView.topAnchor constraintEqualToAnchor:imageView.superview.topAnchor].active = true;
[imageView.bottomAnchor constraintEqualToAnchor:_noticePageControl.bottomAnchor constant:14].active = true;
[imageView.leadingAnchor constraintEqualToAnchor:self.view.leadingAnchor].active = true;
[imageView.trailingAnchor constraintEqualToAnchor:self.view.trailingAnchor].active = true;
```

[iOS ) translatesAutoresizingMaskIntoConstraints](https://zeddios.tistory.com/474)
