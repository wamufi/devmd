---
boostnote:
  createdAt: '2018-11-15T05:02:06.855Z'
  updatedAt: '2019-03-06T03:08:48.045Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: notch bar 색상 변경
  tags:
    - Objective-C
    - UI
    - iOS
  isStarred: false
  linesHighlighted: []
  key: eb23a740-4b35-4510-92c5-e881c9acfe0c
  storage: d112f8ec1e85e056a09d
---

notch bar 색상 변경
---
```objc
UIView *statusBar = (UIView *)[[UIApplication sharedApplication] valueForKey:@"statusBar"];
statusBar.backgroundColor = [UIColor greenColor];
```