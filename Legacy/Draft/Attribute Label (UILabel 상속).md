---
boostnote:
  createdAt: '2018-03-08T06:35:42.293Z'
  updatedAt: '2018-03-08T06:37:15.790Z'
  type: MARKDOWN_NOTE
  folder: 69f287614baf36ef63db
  title: Attribute Label (UILabel 상속)
  tags:
    - Objective-C
  isStarred: false
  key: 534d15bd7dde21eee950
  storage: d112f8ec1e85e056a09d
---

Attribute Label (UILabel 상속)
---
DriveInfoLabel.h
```objc
#import <UIKit/UIKit.h>

@interface DriveInfoLabel : UILabel

@property (nonatomic) NSString *fontName;

@property (nonatomic) int valueFontSize;
@property (nonatomic) NSString *valueFontColor;

@property (nonatomic) int unitFontSize;
@property (nonatomic) NSString *unitFontColor;
@property (nonatomic) NSString *unitString;

- (void)setValueString:(NSString*)valueString;
- (void)updateUnitString:(NSString*)unitString;
- (void)update;

@end
```
```objc
#import "DriveInfoLabel.h"
#import "Utility.h"

@implementation DriveInfoLabel {
    NSString *_valueString;
}

/*
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code
}
*/

- (void)setValueString:(NSString*)valueString {
    _valueString = valueString;
    
    [self update];
}

- (void)updateUnitString:(NSString*)unitString {
    self.unitString = unitString;
    
    [self update];
}

- (void)update {
    if (!_valueString) {
        _valueString = @"";
    }
    
    NSString *text = [_valueString stringByAppendingString:self.unitString];
    NSMutableAttributedString *attributedText = [[NSMutableAttributedString alloc] initWithString:text];
    
    NSRange valueRange = [text rangeOfString:_valueString];
    [attributedText setAttributes:@{NSForegroundColorAttributeName: (self.valueFontColor.length == 6 ? [Utility colorWithHexString:self.valueFontColor] : [UIColor blackColor]),
                                    NSFontAttributeName: [UIFont fontWithName:self.fontName size:self.valueFontSize]}
                            range:valueRange];
    
    NSRange unitRange = [text rangeOfString:self.unitString];
    [attributedText setAttributes:@{NSForegroundColorAttributeName: (self.unitFontColor.length == 6 ? [Utility colorWithHexString:self.valueFontColor] : [UIColor blackColor]),
                                    NSFontAttributeName: [UIFont fontWithName:self.fontName size:self.unitFontSize]}
                            range:unitRange];
    
    self.attributedText = attributedText;

}

@end
```