---
boostnote:
  createdAt: '2019-08-21T09:22:06.259Z'
  updatedAt: '2019-08-22T05:59:41.747Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: notch bar 여부 판단
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 0b6eef99-492d-4b9c-8b6e-2e36c7168806
  storage: d112f8ec1e85e056a09d
---

notch bar 여부 판단
---
iOS 11 버전 미만은 노치가 없다.

```objc
- (BOOL)isHasNotch {
    if (@available(iOS 11.0, *)) {
        UIEdgeInsets safeAreaInsets = [[[UIApplication sharedApplication] keyWindow] safeAreaInsets];
        
        CGFloat bottom = safeAreaInsets.bottom;
        if (bottom > 0) {
            return true;
        } else {
            return false;
        }
    } else {
        return false;
    }
}
```