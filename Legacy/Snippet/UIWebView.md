---
boostnote:
  createdAt: '2019-02-08T05:30:34.602Z'
  updatedAt: '2019-03-06T03:08:10.392Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIWebView
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: a9dfe3f2-66f9-4e9a-9b67-880b74bbac99
  storage: d112f8ec1e85e056a09d
---

UIWebView
---
```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
}

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    
    UINavigationBar *navigationBar = self.navigationController.navigationBar;
    
    navigationBar.translucent = false;
    navigationBar.barTintColor = [UIColor colorWithRed:0.31 green:0.21 blue:0.16 alpha:1.0];
    navigationBar.tintColor = [UIColor whiteColor];
    [navigationBar setTitleTextAttributes:@{NSForegroundColorAttributeName : [UIColor whiteColor]}];
    [navigationBar setShadowImage:[[UIImage alloc] init]];
    
    self.title = @"Crown Pay";
    UIApplication.sharedApplication.statusBarStyle = UIStatusBarStyleLightContent;
    
    [self loadWebView];
}

- (void)loadWebView {
    _webView = [[UIWebView alloc] init];
    [_viewForWebView addSubview:_webView];
    
    [_webView loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"https://www.google.com/"]]];
}

- (void)viewDidLayoutSubviews {
    _webView.frame = _viewForWebView.bounds;
}
```