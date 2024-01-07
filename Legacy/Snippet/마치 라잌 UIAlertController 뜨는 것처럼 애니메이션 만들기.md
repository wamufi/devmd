---
boostnote:
  createdAt: '2020-10-28T02:33:06.821Z'
  updatedAt: '2020-10-28T02:37:55.184Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 마치 라잌 UIAlertController 뜨는 것처럼 애니메이션 만들기
  tags:
    - Animation
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 5044bcd8-4402-46b1-a736-2de337839309
  storage: d112f8ec1e85e056a09d
---

마치 라잌 UIAlertController 뜨는 것처럼 애니메이션 만들기
---
팝업 띄울 때 present 쓰면 백그라운드까지 따라와서 갱장히 거슬렸다. ??? 님 디자이너 아닌데요?
아무튼 그래서 만들어봄. 똑같이는 아니지만 얼추 비슷함.
사이즈가 0에서 1로 가는 애니메이션임.
viewDidAppear 가릴 용도도 있음 ㅋㅋ 근데 굳이 넣어줄 필요는 없으므로 실제 프로젝트에서는 히든 처리함. 깜박거리는거 다 히든 시켜버릴까 ㅅㅂ
태클 걸리면 넣어주지 뭐

부를 ViewController
```swift
let storyboard = UIStoryboard(name: "Popup", bundle: nil)
let viewController = storyboard.instantiateViewController(withIdentifier: "PromotionPopupViewController")
viewController.modalPresentationStyle = .overCurrentContext
present(viewController, animated: false)
```

팝업 ViewController
```swift
func viewDidAppear() {
    containerView.transform = CGAffineTransform(scaleX: 0.01, y: 0.01)
    UIView.animate(withDuration: 0.2, delay: 0, options: .curveEaseOut, animations: {
        self.containerView.transform = .identity
    }, completion: nil)
}

@objc func dismissViewcontroller() {
    containerView.transform = .identity
    UIView.animate(withDuration: 0.2, delay: 0, options: .curveEaseOut) {
        self.containerView.transform = CGAffineTransform(scaleX: 0.01, y: 0.01)
    } completion: { _ in
        self.containerView.isHidden = true
        self.dismiss(animated: false, completion: nil)
    }
}
```

present, dismiss할 때 애니메이션 false 처리 해야한다.