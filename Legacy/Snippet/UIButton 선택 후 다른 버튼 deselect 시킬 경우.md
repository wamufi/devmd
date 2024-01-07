---
boostnote:
  createdAt: '2019-01-09T02:26:47.018Z'
  updatedAt: '2022-07-15T02:23:12.931Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIButton 선택 후 다른 버튼 deselect 시킬 경우
  tags:
    - Objective-C
    - iOS
  isStarred: false
  linesHighlighted: []
  key: 9f6de46d-e545-4064-8ef6-6956a4b768ca
  storage: d112f8ec1e85e056a09d
---

UIButton 선택 후 다른 버튼 deselect 시킬 경우
---
```objc
- (void)setButtonSelected:(UIButton *)currentButton {
    for (UIView *button in currentButton.superview.subviews) {
        if ([button isKindOfClass:[UIButton class]]) {
            [(UIButton *)button setSelected:NO];
        }
    }
    
    [currentButton setSelected:true];
}
```

```swift
// 선택한 버튼만 select, 다른 버튼 deselect
private func setSelectedButton(_ sender: UIButton) {
    guard let superview = sender.superview else { return }
    
    for subview in superview.subviews {
        guard let button = subview as? UIButton else { return }
        button.isSelected = false
    }

    sender.isSelected = true
}
```

```swift
// 같은 레이아웃에 있는 모든 버튼 deselect
private func setDeselectedButton(_ sender: UIButton) {
    guard let superview = sender.superview else { return }

    for subview in superview.subviews {
        guard let button = subview as? UIButton else { return }
        button.isSelected = false
    }
}
```