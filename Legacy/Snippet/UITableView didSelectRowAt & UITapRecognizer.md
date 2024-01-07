---
boostnote:
  createdAt: '2020-07-14T07:42:23.650Z'
  updatedAt: '2020-10-08T00:42:12.198Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView didSelectRowAt & UITapRecognizer
  tags:
    - Swift
    - UITableView
    - UITapRecognizer
  linesHighlighted: []
  isStarred: false
  key: 87e426a8-9292-47b3-8f4c-43885016698c
  storage: d112f8ec1e85e056a09d
---

UITableView didSelectRowAt & UITapRecognizer
---
didSelectRowAt, UITapRecognizer가 겹칠 때
```swift
let tap = UITapGestureRecognizer(target: self, action: #selector(endEditing))
tap.cancelsTouchesInView = false
view.addGestureRecognizer(tap)
```

```swift
tap.cancelsTouchesInView = false
```
가 핵심