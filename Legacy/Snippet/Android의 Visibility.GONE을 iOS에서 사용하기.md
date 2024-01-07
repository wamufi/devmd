---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Android의 Visibility.GONE을 iOS에서 사용하기
  tags:
    - Objective-C
    - UX
    - iOS
  isStarred: false
  createdAt: '2017-09-29T06:29:16.664Z'
  updatedAt: '2022-08-29T08:59:54.530Z'
  linesHighlighted: []
  key: dc6bcf43a157f419355f
  storage: d112f8ec1e85e056a09d
---

Android의 Visibility.GONE을 iOS에서 사용하기
---
1. header file에 사용하고자 하는 NSLayoutConstraint의 property 생성
2. NSLayoutConstraint.constant = 0;
또는
1. header file에 사용하고자 하는 NSLayoutConstraint의 constant 0
2. isActive = true/fase

* Stack View 사용 시에는 hidden 처리

[iOS equivalent for Android View.GONE visibility mode](https://stackoverflow.com/questions/17869268/ios-equivalent-for-android-view-gone-visibility-mode)



```objc
[_superView addConstraint:[NSLayoutConstraint constraintWithItem:_goneView attribute:NSLayoutAttributeHeight relatedBy:NSLayoutRelationEqual toItem:nil attribute:NSLayoutAttributeNotAnAttribute multiplier:1 constant:0]];
[_superView removeConstraint:_goneViewHeight];
```