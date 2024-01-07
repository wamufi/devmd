---
boostnote:
  createdAt: '2019-11-26T05:30:30.268Z'
  updatedAt: '2019-11-26T06:37:10.880Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIVisualEffectView
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: db10b250-c9dd-4d77-91bb-c8f8c957c845
  storage: d112f8ec1e85e056a09d
---

UIVisualEffectView
---
**iOS 9 음악 앱 같은 블러효과를 주기 위해 사용**


imageView < visualEffectView < imageView

```objc
UIImage *image = [UIImage imageWithData: [NSData dataWithContentsOfURL:url]];
_imageView.image = image;
_imageView.contentMode = UIViewContentModeScaleAspectFill;

UIVisualEffect *visualEffect = [UIBlurEffect effectWithStyle:UIBlurEffectStyleLight];
UIVisualEffectView *visualEffectView = [[UIVisualEffectView alloc] initWithEffect:visualEffect];
visualEffectView.frame = _imageView.frame;
visualEffectView.autoresizingMask = UIViewAutoresizingFlexibleHeight;
[_imageView addSubview:visualEffectView];

UIImageView *effectImageView = [[UIImageView alloc] initWithImage:image];
effectImageView.contentMode = UIViewContentModeScaleAspectFit;
effectImageView.frame = _imageView.frame;
[visualEffectView.contentView addSubview:effectImageView];
```