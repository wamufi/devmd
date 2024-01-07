---
boostnote:
  createdAt: '2021-02-25T09:19:11.955Z'
  updatedAt: '2021-02-25T09:21:00.758Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Custom URL Scheme 적용할 때 didFailProvisionalNavigation 에러 정리
  tags:
    - WKWebView
    - Swift
  linesHighlighted: []
  isStarred: false
  key: 93a86ed0-8c7e-49a7-bff6-36d085883bda
  storage: d112f8ec1e85e056a09d
---

Custom URL Scheme 적용할 때 didFailProvisionalNavigation 에러 정리
---
에러는 1002, Unsupported URL

```swift
webView.navigationDelegate = self

func webView(_ webView: WKWebView, decidePolicyFor navigationAction: WKNavigationAction, decisionHandler: @escaping (WKNavigationActionPolicy) -> Void) {

    // if the request is a non-http(s) schema, then have the UIApplication handle
    // opening the request
    if let url = navigationAction.request.url, !url.absoluteString.hasPrefix("http://"), !url.absoluteString.hasPrefix("https://"), UIApplication.shared.canOpenURL(url) {

        // have UIApplication handle the url (sms:, tel:, mailto:, ...)
        UIApplication.shared.open(url, options: [:], completionHandler: nil)

        // cancel the request (handled by UIApplication)
        decisionHandler(.cancel)
    }
    else {
        // allow the request
        decisionHandler(.allow)
    }
}
```

https://stackoverflow.com/questions/47040471/wkwebview-not-opening-custom-url-scheme-js-opens-custom-scheme-link-in-new-wind