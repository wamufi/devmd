---
boostnote:
  createdAt: '2018-10-29T07:03:03.307Z'
  updatedAt: '2019-03-06T03:08:52.556Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: '두 가지 색의 Navigation Controller, Status Bar'
  tags:
    - Objective-C
    - UI
    - iOS
  isStarred: false
  linesHighlighted: []
  key: 1e6e9239-23b4-4377-9c81-52379ecedea9
  storage: d112f8ec1e85e056a09d
---

두 가지 색의 Navigation Controller, Status Bar
---
FirstViewController.m
```objc
- (void)viewWillAppear:(BOOL)animated {
    self.navigationController.navigationBar.barTintColor = [UIColor colorWithRed:0.74 green:0.62 blue:0.45 alpha:1.0];
    self.navigationController.navigationBar.tintColor = [UIColor colorWithRed:0.31 green:0.21 blue:0.16 alpha:1.0];
    UIApplication.sharedApplication.statusBarStyle = UIStatusBarStyleDefault;
}
```

SecondViewController.m

```objc
- (void)viewWillAppear:(BOOL)animated {
    self.navigationController.navigationBar.translucent = false;
    self.navigationController.navigationBar.barTintColor = [UIColor colorWithRed:0.31 green:0.21 blue:0.16 alpha:1.0];
    self.navigationController.navigationBar.tintColor = [UIColor whiteColor];
    UIApplication.sharedApplication.statusBarStyle = UIStatusBarStyleLightContent;
}
```