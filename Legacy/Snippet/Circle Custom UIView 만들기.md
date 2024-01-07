---
boostnote:
  createdAt: '2021-12-15T06:33:14.116Z'
  updatedAt: '2021-12-16T06:32:52.172Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Circle Custom UIView 만들기
  tags:
    - iOS
    - UIView
    - Swift
    - Custom
  linesHighlighted: []
  isStarred: false
  key: 42cbafb1-3564-4762-a066-0b36ba461199
  storage: d112f8ec1e85e056a09d
---

Circle Custom UIView 만들기
---
```swift
// 원 그리기
layer.cornerRadius = frame.width / 2
clipsToBounds = true

override func draw(_ rect: CGRect) {
    // 원 그리기
    let path = UIBezierPath(ovalIn: rect)
    let shapeLayer = CAShapeLayer()
    shapeLayer.path = path.cgPath
    shapeLayer.fillColor = circleColor.cgColor
    layer.addSublayer(shapeLayer)

    setupView()
}
```