---
boostnote:
  createdAt: '2020-10-05T03:07:26.612Z'
  updatedAt: '2020-10-05T04:31:26.742Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Create a UILabel programmatially in UIView with Center
  tags:
    - Swift
    - programmatially
  linesHighlighted: []
  isStarred: false
  key: 06534184-2c10-4ccf-a4c2-68e2c8eb206a
  storage: d112f8ec1e85e056a09d
---

Create a UILabel programmatially in UIView with Center
---
UIView의 센터에 UILabel을 만들어보자 코드로
```swift
func printMissionTitle(string: String) {
    let label = UILabel()
    label.text = string
    label.textColor = UIColor(named: "Primary")
    label.textAlignment = .center

    if string.count < 15 {
        label.font = UIFont.boldSystemFont(ofSize: 22)
    } else {
        label.font = UIFont.boldSystemFont(ofSize: 18)
    }

    missionTitleView.addSubview(label)
    label.frame.size = CGSize(width: label.intrinsicContentSize.width, height: label.intrinsicContentSize.height)
    label.center = CGPoint(x: label.superview!.frame.width / 2 - label.superview!.frame.origin.x / 2, y: label.superview!.frame.height / 2)
}
```

label의 center 잡을 때 x 좌표에 origin.x를 안 넣으면 우측으로 밀려서 나온다. superview의 origin.x가 0이면 상관 없을 것 같지만 0이 아니라면 저렇게 넣어줘야 한다.