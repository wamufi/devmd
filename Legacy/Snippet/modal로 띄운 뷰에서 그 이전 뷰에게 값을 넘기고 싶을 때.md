---
boostnote:
  createdAt: '2020-10-31T10:35:15.107Z'
  updatedAt: '2022-01-06T02:58:07.836Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: modal로 띄운 뷰에서 그 이전 뷰에게 값을 넘기고 싶을 때
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 93dcf2d0-75f8-4738-a8d2-4969488b4f83
  storage: d112f8ec1e85e056a09d
---

modal로 띄운 뷰에서 그 이전 뷰에게 값을 넘기고 싶을 때
---
```swift
@IBAction func dismiss() {
    if let presenter = presentingViewController?.children.last as? ReceiptListViewController {
        presenter.dateChanged()
    }
    dismiss(animated: true, completion: nil)
}
```

보통은 그냥 `presenter = presentingViewController` 해도 된다. 쟤는 좀 특수한 경우.

+)
ContainerViewController를 걷어냈었나?
이전 뷰가 modal로 떴을 때는 `presenter = presentingViewController?.children.last`로 뜨고, push로 떴을 때는 `presenter = presentingViewController`가 되었다.
시발 네비게이션 미쳐버리겠다.

++)
TabBar 사용 시에는
```swift
if let presenter = (self.presentingViewController as? UITabBarController)?.selectedViewController?.children.last as? LottoWinHistoryViewController {
    presenter.test()
}
```