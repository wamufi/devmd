---
boostnote:
  createdAt: '2022-09-13T01:58:47.056Z'
  updatedAt: '2022-09-13T01:59:35.860Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableViewCell의 setSelected가 여러번 불릴 때
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 75a1b4d6-b1ac-4722-91a2-4aecca28a711
  storage: d112f8ec1e85e056a09d
---

UITableViewCell의 setSelected가 여러번 불릴 때
---
```swift
override func setSelected(_ selected: Bool, animated: Bool) {
    if isSelected == selected { return } // 이걸 넣어주면 된다
    super.setSelected(selected, animated: animated)

    if !selected {
        answerView.isHidden = true
        answerViewHeightZero.isActive = true
        descLabel.isHidden = true
        arrowImageView.transform = .identity
    }
}
```

https://stackoverflow.com/questions/5401226/uitableviewcell-selector-setselectedanimated-gets-called-many-times
