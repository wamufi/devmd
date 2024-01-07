---
boostnote:
  createdAt: '2022-10-17T07:05:29.582Z'
  updatedAt: '2022-10-17T07:07:36.425Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: WKWebView dynamic height
  tags:
    - Swift
    - iOS
    - WKWebView
  linesHighlighted: []
  isStarred: false
  key: f6e9099e-6373-4ecb-81b9-d1ca673f96c1
  storage: d112f8ec1e85e056a09d
---

WKWebView dynamic height
---
```swift
private func adjustWebViewHeight() {
//        let script = "document.documentElement.scrollHeight"
    let script = "document.querySelector('.pord-tab-detail').scrollHeight"

    DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) { [self] in
        detailWebView.evaluateJavaScript("document.readyState", completionHandler: { (complete, error) in
            if complete != nil {
                self.detailWebView.evaluateJavaScript(script, completionHandler: { (height, error) in
                    if error == nil {
                        self.detailWebViewHeightFloat = (height as? CGFloat ?? 0)
                        self.detailWebView.snp.updateConstraints { make in
                            make.height.equalTo(self.detailWebViewHeightFloat)
                        }
                    }
                })
            }
        })
    }
}
```

`DispatchQueue.main.asyncAfter(deadline:)`은 스크립트가 안 먹을 경우에 사용
- 이번 경우에는 콜을 했는데 높이 변하는 인터랙션을 한 번 거쳐야 작동이 되어서 추가함