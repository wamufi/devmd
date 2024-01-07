---
boostnote:
  createdAt: '2021-01-20T06:55:38.158Z'
  updatedAt: '2021-01-20T07:15:11.905Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Custom User Agent
  tags:
    - Android
    - Java
    - WebKit
    - iOS
    - Objective-C
    - WebView
  linesHighlighted: []
  isStarred: false
  key: 0d8920aa-5ba4-4ae2-8b4e-97931fa8628e
  storage: d112f8ec1e85e056a09d
---

Custom User Agent
---
```java
webView.getSettings().setUserAgentString(webView.settings.userAgentString + " tookApp");
```

```objc 
configuration = [[WKWebViewConfiguration alloc] init];
configuration.applicationNameForUserAgent = [NSString stringWithFormat:@"%@%@", configuration.applicationNameForUserAgent, @"tookApp"];

_webView = [[WKWebView alloc] initWithFrame:CGRectZero configuration:configuration];
```