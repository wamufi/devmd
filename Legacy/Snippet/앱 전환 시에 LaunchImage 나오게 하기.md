---
boostnote:
  createdAt: '2019-02-19T00:44:21.732Z'
  updatedAt: '2019-03-06T03:07:27.506Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 앱 전환 시에 LaunchImage 나오게 하기
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 59eee2c3-ad4e-4582-8351-b16ef0277294
  storage: d112f8ec1e85e056a09d
---

앱 전환 시에 LaunchImage 나오게 하기
---
```objc
- (void)applicationWillResignActive:(UIApplication *)application {
    // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
    // Use this method to pause ongoing tasks, disable timers, and invalidate graphics rendering callbacks. Games should use this method to pause the game.
    UIImageView *imageView = [[UIImageView alloc] initWithFrame:self.window.bounds];
    imageView.tag = 100;
    imageView.image = [UIImage imageNamed:@"LaunchImage"];
    [UIApplication.sharedApplication.keyWindow.subviews.lastObject addSubview:imageView];
}

- (void)applicationDidBecomeActive:(UIApplication *)application {
    // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
    UIImageView *imageView = (UIImageView *)[UIApplication.sharedApplication.keyWindow.subviews.lastObject viewWithTag:100];
    [imageView removeFromSuperview];
}
```