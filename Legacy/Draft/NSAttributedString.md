---
boostnote:
  createdAt: '2018-03-14T07:23:34.298Z'
  updatedAt: '2018-10-25T05:32:36.337Z'
  type: MARKDOWN_NOTE
  folder: 69f287614baf36ef63db
  title: NSAttributedString
  tags:
    - Objective-C
  isStarred: false
  key: faf24a6dcdd593dd449c
  storage: d112f8ec1e85e056a09d
---

NSAttributedString
---
```objc
- (void)setLeftLabelByRemainCount:(NSString *)remainCount {
    int remainCountInt = [remainCount intValue];
    if (remainCountInt < 0) {
        remainCountInt = 0;
    }
    
    NSString *string1 = [NSString stringWithFormat:@"Free services left:"];
    NSString *string2 = [NSString stringWithFormat:@"%d times", remainCountInt];
    NSString *string3 = @"Billing will begin after using up the free services.";
    NSString *string = [NSString stringWithFormat:@"%@ %@\n%@", string1, string2, string3];
    
    if ([_leftLabel respondsToSelector:@selector(setAttributedText:)]) {
        UIColor *timesColor = [UIColor colorWithRed:0 green:0.666 blue:0.823 alpha:1];
        NSDictionary *attribs = @{NSForegroundColorAttributeName: _leftLabel.textColor, NSFontAttributeName: _leftLabel.font};
        NSMutableAttributedString *attributedText = [[NSMutableAttributedString alloc] initWithString:string attributes:attribs];
        NSRange string2Range = [string rangeOfString:string2];
        [attributedText setAttributes:@{NSForegroundColorAttributeName:timesColor} range:string2Range];
        
        _leftLabel.attributedText = attributedText;
    }
}
```