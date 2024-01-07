---
boostnote:
  createdAt: '2019-11-14T06:16:23.737Z'
  updatedAt: '2019-11-14T06:21:49.976Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 앱 실행될 때마다 alert 창 띄우기
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: b25f9351-9071-4b6b-a9f0-a8edd129571e
  storage: d112f8ec1e85e056a09d
---

앱 실행될 때마다 alert 창 띄우기
---
백그라운드에서 포그라운드로 올라올 때도 포함

AppDelegate.m
```objc
- (void)applicationDidBecomeActive:(UIApplication *)application {
    // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
    
    [self showAlert];
}

- (void)showAlert {
    __block UIWindow *alertWindow = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    alertWindow.rootViewController = [UIViewController new];
    alertWindow.windowLevel = UIWindowLevelAlert + 1
    
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"Title" message:@"Message" preferredStyle:UIAlertControllerStyleAlert];
    UIAlertAction *okAction = [UIAlertAction actionWithTitle:NSLocalizedString(@"OK",@"confirm") style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        alertWindow.hidden = true;
        alertWindow = nil;
        
        // [alert dismissViewControllerAnimated:YES completion:nil];
        }
    }];
    
    [alertController addAction:okAction];
    
    [alertWindow makeKeyAndVisible];
    [alertWindow.rootViewController presentViewController:alert animated:true completion:nil];
}
```