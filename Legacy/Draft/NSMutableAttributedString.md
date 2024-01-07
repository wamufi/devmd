---
boostnote:
  createdAt: '2019-01-10T09:32:25.709Z'
  updatedAt: '2019-01-10T09:33:47.179Z'
  type: MARKDOWN_NOTE
  folder: 69f287614baf36ef63db
  title: NSMutableAttributedString
  tags:
    - Objective-C
  isStarred: false
  key: 017603c9-9d52-44e8-b438-59b3b75d0654
  storage: d112f8ec1e85e056a09d
---

NSMutableAttributedString
---
```objc
NSString *restCrownLabelString = [NSString stringWithFormat:@"Crown %@개를 더 모으시면/n무료음료쿠폰이 발급됩니다.", restCrown];
NSMutableAttributedString *restCrownLabelText = [[NSMutableAttributedString alloc] initWithString:restCrownLabelString];
NSMutableParagraphStyle *centerParagraph = [NSMutableParagraphStyle new];
[centerParagraph setLineSpacing:5.0];
centerParagraph.alignment = NSTextAlignmentCenter;
[restCrownLabelText addAttribute:NSParagraphStyleAttributeName value:centerParagraph range:NSMakeRange(0, [restCrownLabelString length])];
[restCrownLabelText addAttribute:NSFontAttributeName value:[UIFont fontWithName:@"NanumGothicBold" size:16] range:[restCrownLabelString rangeOfString:@"무료음료쿠폰"]];

_restCrownLabel.attributedText = restCrownLabelText;
```

https://qiita.com/shu223/items/c2eb639976154a491d19