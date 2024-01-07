---
boostnote:
  createdAt: '2022-07-06T09:15:08.846Z'
  updatedAt: '2022-07-06T09:15:32.306Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIButton 배경색
  tags:
    - UIButton
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 95643f19-ce43-4f54-a735-decacdc0afb8
  storage: d112f8ec1e85e056a09d
---

UIButton 배경색
---
```swift
extension UIButton {
    func setBackgroundColor(_ color: UIColor, for state: UIControl.State) {
        UIGraphicsBeginImageContext(CGSize(width: 1.0, height: 1.0))
        guard let context = UIGraphicsGetCurrentContext() else { return }
        context.setFillColor(color.cgColor)
        context.fill(CGRect(x: 0.0, y: 0.0, width: 1.0, height: 1.0))

        let backgroundImage = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()

        setBackgroundImage(backgroundImage, for: state)
    }
}
```