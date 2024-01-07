---
boostnote:
  createdAt: '2018-10-29T05:21:11.676Z'
  updatedAt: '2019-03-06T03:08:57.003Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: '전체 Navigation Bar, Status Bar 색상 변경'
  tags:
    - Objective-C
    - UI
    - iOS
  isStarred: false
  linesHighlighted: []
  key: ead3b869-73e5-46f9-9323-df672106279c
  storage: d112f8ec1e85e056a09d
---

전체 Navigation Bar, Status Bar 색상 변경
---
AppDelegate.m

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
- 
    // Navigation Bar 색상 변경
    UINavigationBar.appearance.barTintColor = [UIColor colorWithRed:0.31 green:0.21 blue:0.16 alpha:1.0];
    // Navigation Bar Items 색상 변경
    UINavigationBar.appearance.tintColor = [UIColor whiteColor];
    // Navigation Bar Title 색상 변경
    [UINavigationBar.appearance setTitleTextAttributes:@{NSForegroundColorAttributeName : [UIColor whiteColor]}];
    [self.navigationController.navigationBar setTitleTextAttributes:@{NSForegroundColorAttributeName : [UIColor whiteColor]}];
    // Status Bar의 text 색상 변경
    UIApplication.sharedApplication.statusBarStyle = UIStatusBarStyleLightContent;
    
    return YES;
}
```

Info.plist
```
View controller-based status bar appearance = NO
```