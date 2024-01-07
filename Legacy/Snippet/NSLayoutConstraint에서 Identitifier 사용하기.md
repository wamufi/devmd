---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: NSLayoutConstraint에서 Identitifier 사용하기
  tags:
    - Objective-C
    - iOS
  isStarred: false
  createdAt: '2017-09-29T06:24:24.688Z'
  updatedAt: '2019-03-06T03:09:47.267Z'
  linesHighlighted: []
  key: 787241fd9e16fac58fe6
  storage: d112f8ec1e85e056a09d
---

NSLayoutConstraint에서 Identitifier 사용하기
---
1. Constraint -> Utilites Panel -> Attributes Inspector -> Identitifer 지정
```objc
UIView *subview = [[UIView alloc] init];
NSLayoutConstraint *constraint = [[NSLayoutConstraint alloc] init];
for (subview in self.view.subviews) {
    for (constraint in subview.constraints()) {
        if ([constraint.identitifer isEqualToString:@"Identitifier"]) {
            // constraint 수정
        }
    }
}
```
[Is there a way to add an identifier to Auto Layout Constraints in Interface Builder?](https://stackoverflow.com/a/31831576)