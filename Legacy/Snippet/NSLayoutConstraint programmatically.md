---
boostnote:
  createdAt: '2020-09-10T06:30:44.478Z'
  updatedAt: '2021-07-15T05:06:21.041Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: NSLayoutConstraint programmatically
  tags:
    - NSLayoutConstraint
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 495066a2-38df-4abd-986c-a5e413a80eb8
  storage: d112f8ec1e85e056a09d
---

NSLayoutConstraint programmatically
---
`translatesAutoresizingMaskIntoConstraints = false` 넣어주자


```swift
label.trailingAnchor.constraint(equalTo: label.superview!.trailingAnchor, constant: 0).isActive = true
label.centerYAnchor.constraint(equalTo: label.superview!.centerYAnchor, constant: 0).isActive = true
        
// 에러남 이유 모름 똑같이 superview인데 왜? view hierarchy 관련 에러 남
//        label.addConstraint(NSLayoutConstraint(item: label, attribute: .centerY, relatedBy: .equal, toItem: pointLabel.superview, attribute: .centerY, multiplier: 1, constant: 0))
//        label.addConstraint(NSLayoutConstraint(item: label, attribute: .trailing, relatedBy: .equal, toItem: pointLabel.superview, attribute: .trailing, multiplier: 1, constant: 0))
```

Visual Format
```swift
let views = ["label": textLabel]
let horizontalConstraints = NSLayoutConstraint.constraints(withVisualFormat: "H:|-30-[label]-30-|", metrics: nil, views: views)
let verticalConstraints = NSLayoutConstraint.constraints(withVisualFormat: "V:|-100-[label]-100-|", metrics: nil, views: views)
addConstraints(horizontalConstraints)
addConstraints(verticalConstraints)
```


또 있을걸?
이거 관련 framework도 있는데 안 써봤다.