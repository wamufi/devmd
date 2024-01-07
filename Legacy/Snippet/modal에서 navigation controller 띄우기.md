---
boostnote:
  createdAt: '2020-08-06T04:58:20.645Z'
  updatedAt: '2020-08-06T04:59:26.690Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: modal에서 navigation controller 띄우기
  tags:
    - Swift
    - UI
    - iOS
  linesHighlighted: []
  isStarred: false
  key: d043fd91-0d4a-4f7d-905a-60b9989faecf
  storage: d112f8ec1e85e056a09d
---

modal에서 navigation controller 띄우기
---
```swift
@IBAction func moveAction(_ sender: Any) {
    let storyboard = UIStoryboard(name: "MyH", bundle: nil)
    let viewController = storyboard.instantiateViewController(withIdentifier: "MyMobileCardViewController") as! MyMobileCardViewController

    let navigationController = UINavigationController(rootViewController: viewController)
    navigationController.modalPresentationStyle = .overCurrentContext
    self.present(navigationController, animated: true, completion: nil)
}
```