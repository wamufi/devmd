---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: NSLayoutConstraint의 Multiplier 수정하기
  tags:
    - Objective-C
    - iOS
  isStarred: false
  createdAt: '2017-09-29T06:21:42.664Z'
  updatedAt: '2019-03-27T05:25:06.809Z'
  linesHighlighted: []
  key: 3a86f22244af5e9bada8
  storage: d112f8ec1e85e056a09d
---

NSLayoutConstraint의 Multiplier 수정하기
---
multiplier는 readonly이기 때문에 constraint.multiplier로 수정이 안 된다.
새로 만들고 전에 쓰던 constraint를 지우고 덮어쓰자.

```objc
- (void)changeMultiplier:(NSLayoutConstraint *)constraint to:(CGFloat)value in:(UIView *)view {
    NSLayoutConstraint *newConstraint = [[NSLayoutConstraint alloc] init];
    newConstraint = [NSLayoutConstraint constraintWithItem:constraint.firstItem attribute:constraint.firstAttribute relatedBy:constraint.relation toItem:constraint.secondItem attribute:constraint.secondAttribute multiplier:value constant:constraint.constant];
    [view removeConstraint:constraint];
    [view addConstraint:newConstraint];
}
```

[auto layout modify multiplier of constriaint programmatically](https://stackoverflow.com/a/30722251)