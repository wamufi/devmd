---
boostnote:
  createdAt: '2019-02-19T08:27:46.627Z'
  updatedAt: '2019-08-13T08:36:50.173Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Push
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 2019da45-c8f4-4bd5-a692-94f861b51b2f
  storage: d112f8ec1e85e056a09d
---

Push
---
[iOS Push 수신할 때 앱의 3가지 상태 확인](https://www.developer-leby.kim/m/148)
[iphone - Get push notification while App in foreground iOS - Stack Overflow](https://stackoverflow.com/questions/14872088/get-push-notification-while-app-in-foreground-ios)
[[SWIFT] APNS, 푸시에 관한 설정(receive event, click(tap) event)](https://g-y-e-o-m.tistory.com/87)

```objc
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    completionHandler(UNNotificationPresentationOptionAlert);
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    if (userInfo[@"aps"] != nil) {
        completionHandler(UIBackgroundFetchResultNewData);
    } else {
        completionHandler(UIBackgroundFetchResultNoData);
    }
}
```

```objc
- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(drawBadge) name:UIApplicationDidBecomeActiveNotification object:nil];
}

- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```