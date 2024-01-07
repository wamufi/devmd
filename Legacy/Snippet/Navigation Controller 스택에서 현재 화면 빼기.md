---
boostnote:
  createdAt: '2020-09-07T02:30:06.454Z'
  updatedAt: '2020-09-07T02:36:17.933Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Navigation Controller 스택에서 현재 화면 빼기
  tags:
    - Navigation
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 1f028bd7-fab8-42ba-9344-597c47066a2c
  storage: d112f8ec1e85e056a09d
---

Navigation Controller 스택에서 현재 화면 빼기
---
A -> B -> C -> B -> C -> A
A -> B -> C -> B -> A
이렇게 가고 싶을 경우에 (B/C를 같은 stack이라고 여길 경우, 마치 한 탭에서 왔다갔다 한다고 여길 경우) back button을 눌렀을 경우 unwind 시켜버리면 좋겠지만... 모종의 이유로 까만 화면만 나온다. 아마 ```willMove(to parent: UIViewController)```에서 뭔가 영향이 있는 듯

그래서 그래서
나는 B/C에서 이렇게 해주었다

```swift
@IBAction func pastHTalkAction(_ sender: Any) {
    let storyboard = UIStoryboard(name: "Talk", bundle: nil)
    let viewController = storyboard.instantiateViewController(withIdentifier: "HTalkListViewController") as! HTalkListViewController

    navigationController?.pushViewController(viewController, animated: false)
    navigationController?.viewControllers.remove(at: (navigationController?.viewControllers.count)! - 2) // back buttom 처리를 위해 navcontroller stack에서 현 viewcontroller 제거
}
```

```swift
navigationController?.viewControllers.remove(at: (navigationController?.viewControllers.count)! - 2)
```
이 것 이 핵 심

navigation stack에 쌓여있는 애들 중에 지금 그러니까 B -> C에서는 B, C -> B에서는 C를 이동할 때 제거 해버리는 것!

네 이렇게 처리했습니다...
시발 이렇게 까지 해야되나 자괴감들어 ㅡㅡ
