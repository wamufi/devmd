---
boostnote:
  createdAt: '2020-10-27T03:00:12.988Z'
  updatedAt: '2020-10-27T03:01:26.424Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIView에서 UIViewController 가져오기
  tags: []
  linesHighlighted: []
  isStarred: false
  key: f7269232-fdf5-4631-ba90-bc89cbb1b7b1
  storage: d112f8ec1e85e056a09d
---

UIView에서 UIViewController 가져오기
---
```swift
viewController(self)
```

```swift
func viewController(_ view: UIView) -> UIViewController {
    var responder: UIResponder? = view
    while !(responder is UIViewController) {
        responder = responder?.next
        if nil == responder {
            break
        }
    }
    return (responder as? UIViewController)!
}
```