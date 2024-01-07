---
boostnote:
  createdAt: '2019-10-17T09:05:09.717Z'
  updatedAt: '2019-10-17T09:27:49.421Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: iOS 13 이상에서 status bar만 background color 변경
  tags:
    - Objective-C
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 4d6eb45a-d8d1-4a05-8194-6a4ee541e6d1
  storage: d112f8ec1e85e056a09d
---

iOS 13 이상에서 status bar만 background color 변경
---
```objc
if (@available(iOS 13.0, *)) { 
    UIApplication *app = [UIApplication sharedApplication];
    CGFloat statusBarHeight = app.statusBarFrame.size.height;
        
    UIView *statusBarView = [[UIView alloc] init];
    statusBarView.backgroundColor = [UIColor colorWithRed:0.31 green:0.21 blue:0.16 alpha:1.0];
    [self.view addSubview:statusBarView];
        
    statusBarView.translatesAutoresizingMaskIntoConstraints = false;
    [statusBarView.heightAnchor constraintEqualToConstant:statusBarHeight].active = true;
    [statusBarView.widthAnchor constraintEqualToAnchor:self.view.widthAnchor multiplier:1].active = true;
    [statusBarView.topAnchor constraintEqualToAnchor:self.view.topAnchor].active = true;
    [statusBarView.centerXAnchor constraintEqualToAnchor:self.view.centerXAnchor].active = true;
} else {
    statusBar = (UIView *)[[UIApplication sharedApplication] valueForKey:@"statusBar"];
    statusBar.backgroundColor = [UIColor colorWithRed:0.31 green:0.21 blue:0.16 alpha:1.0];
}
```

```swift
if #available(iOS 13.0, *) {
    let app = UIApplication.shared
    let statusBarHeight: CGFloat = app.statusBarFrame.size.height
    
    let statusbarView = UIView()
    statusbarView.backgroundColor = UIColor.red
    view.addSubview(statusbarView)
  
    statusbarView.translatesAutoresizingMaskIntoConstraints = false
    statusbarView.heightAnchor
        .constraint(equalToConstant: statusBarHeight).isActive = true
    statusbarView.widthAnchor
        .constraint(equalTo: view.widthAnchor, multiplier: 1.0).isActive = true
    statusbarView.topAnchor
        .constraint(equalTo: view.topAnchor).isActive = true
    statusbarView.centerXAnchor
        .constraint(equalTo: view.centerXAnchor).isActive = true
  
} else {
    let statusBar = UIApplication.shared.value(forKeyPath: "statusBarWindow.statusBar") as? UIView
    statusBar?.backgroundColor = UIColor.red
}
```

https://freakycoder.com/ios-notes-13-how-to-change-status-bar-color-1431c185e845