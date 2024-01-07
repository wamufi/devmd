---
boostnote:
  createdAt: '2020-10-17T12:12:44.979Z'
  updatedAt: '2022-07-01T07:40:56.153Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: dismiss 후 present
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: b4b4e9f2-aebd-4ff3-bcfb-d7169976d25f
  storage: d112f8ec1e85e056a09d
---

dismiss 후 present
---
```swift
guard let presentingViewController = presentingViewController else { return }

dismiss(animated: true) {
    presentingViewController.present(navigationController, animated: true, completion: nil)
}
```

+) 
```swift
if let presenter = presentingViewController.children.last as? AlphabetExchangeViewController {
}
```
이런 것도 있다. 이런거는 presentingViewController가 다 최상위 뷰 컨트롤러 일 때 썼다.
예를 들어 ContainerViewController라던가

[[Swift] dismiss 한 뒤 바로 present 하는 방법](https://developer-fury.tistory.com/56)

++)
TabBar 사용 시에는
```swift
private func dismiss() {
    if let presenter = (self.presentingViewController as? UITabBarController)?.selectedViewController?.children.last as? LottoWinHistoryViewController {
        dismiss(animated: true) {
            if #available(iOS 15.0, *) {
                presenter.showSheetPresentationController()
            } else {
                // Fallback on earlier versions
            }
        }
    } else {
        dismiss(animated: true, completion: nil)
    }
}
```