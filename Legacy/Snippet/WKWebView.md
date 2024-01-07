---
boostnote:
  createdAt: '2019-02-09T09:24:04.557Z'
  updatedAt: '2019-03-06T03:08:03.641Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: WKWebView
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 918b79aa-1cd8-4433-aa61-2e8f3a4cddb3
  storage: d112f8ec1e85e056a09d
---

WKWebView
---
iOS 11.0 미만 지원할 때 WKWebView는 storyboard 사용을 못한다. 19/2/9 기준
[WKWebView](https://aircook.tistory.com/entry/WKWebView)

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    
    configuration = [[WKWebViewConfiguration alloc] init];
    jsctrl = [[WKUserContentController alloc] init];

    // 자바스크립트 -> ios에 사용될 핸들러 이름을 추가해줍니다.
    [jsctrl addScriptMessageHandler:self name:@"pbs"];
    [configuration setUserContentController:jsctrl];

    _webView = [[WKWebView alloc] initWithFrame:CGRectZero configuration:configuration];
    _webView.translatesAutoresizingMaskIntoConstraints = false;
    _webView.UIDelegate = self;
    _webView.navigationDelegate = self;
    
    [self.view addSubview:_webView];
    [self setWebViewConstraints];

    NSURL *url = [[NSURL alloc] initWithString:_urlStr];
    NSURLRequest *request = [[NSURLRequest alloc] initWithURL:url];
    //NSMutableURLRequest *request = [[NSMutableURLRequest alloc] initWithURL:url];
    //request.HTTPMethod = @"POST";
    //request.HTTPBody = @"111";
    [_webView loadRequest:request];
}

- (void)setWebViewConstraints {
    if (@available(iOS 11.0, *)) {
        UILayoutGuide *safeArea = self.view.safeAreaLayoutGuide;
        [_webView.leadingAnchor constraintEqualToAnchor:safeArea.leadingAnchor].active = true;
        [_webView.trailingAnchor constraintEqualToAnchor:safeArea.trailingAnchor].active = true;
        [_webView.topAnchor constraintEqualToAnchor:safeArea.topAnchor].active = true;
        [_webView.bottomAnchor constraintEqualToAnchor:safeArea.bottomAnchor].active = true;
    } else {
        [_webView.leadingAnchor constraintEqualToAnchor:self.view.leadingAnchor].active = true;
        [_webView.trailingAnchor constraintEqualToAnchor:self.view.trailingAnchor].active = true;
        [_webView.topAnchor constraintEqualToAnchor:self.topLayoutGuide.bottomAnchor].active = true;
        [_webView.bottomAnchor constraintEqualToAnchor:self.bottomLayoutGuide.topAnchor].active = true;
    }
}

```