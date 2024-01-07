---
boostnote:
  createdAt: '2020-03-30T08:07:43.732Z'
  updatedAt: '2020-08-12T04:40:38.503Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: view 안에 있는 특정 클래스(이미지뷰/텍스트뷰) 가져와서 변경하기
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 4c78ca15-6ea7-4bfb-af21-bdf354fc5bc6
  storage: d112f8ec1e85e056a09d
---

view 안에 있는 특정 클래스(이미지뷰/텍스트뷰) 가져와서 변경하기
---
```objc
- (void)changeTextColor {
    UIColor *textColor = [UIColor whiteColor];
    _labels = [NSMutableArray new];
    [self getAllLabelsInView:self.view];

    for (UILabel *label in _labels) {
        label.textColor = textColor;
    }
}

- (void)getAllLabelsInView:(UIView *)view {
    for (id subview in view.subviews) {
        if ([subview isKindOfClass:[UILabel class]]) {
            UILabel *label = (UILabel *)subview;
            [_labels addObject:label];
        } else {
            [self getAllLabelsInView:subview];
        }
    }
}

- (void)changeImageColor {
    UIColor *iconColor = [UIColor colorWithRed:0.64 green:0.52 blue:0.35 alpha:1.0];
    _iconImages = [NSMutableArray new];
    [self getAllImagesInView:_viewAfterLogin];
    [self getAllImagesInView:_underLayoutView];
    NSLog(@"!!! iconImages: %@", _iconImages);
    
    for (UIImageView *imageView in _iconImages) {
        imageView.image = [imageView.image imageWithRenderingMode:UIImageRenderingModeAlwaysTemplate];
        imageView.tintColor = iconColor;
    }
    
    UIImageView *progressImageView = [_iconImages objectAtIndex:1];
    progressImageView.image = [progressImageView.image imageWithRenderingMode:UIImageRenderingModeAlwaysTemplate];
    progressImageView.tintColor = [UIColor whiteColor];
}

- (void)getAllImagesInView:(UIView *)view {
    for (id subview in view.subviews) {
        if ([subview isKindOfClass:[UIImageView class]]) {
            UIImageView *imageView = (UIImageView *)subview;
            NSLog(@"!!! imageView: %@", imageView);

            [_iconImages addObject:imageView];
        } else {
            [self getAllImagesInView:subview];
        }
    }
}
```

```swift
func getButtonsIn(_ view: UIView) {
    for subview in view.subviews {
        if subview is UIButton {
            buttons.append(button)
        } else {
            getButtonIn(subview)
        }
    }
}
```