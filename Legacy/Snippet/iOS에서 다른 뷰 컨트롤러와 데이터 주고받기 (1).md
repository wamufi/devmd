---
boostnote:
  createdAt: '2018-12-11T07:26:01.034Z'
  updatedAt: '2019-03-06T03:08:34.894Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: iOS에서 다른 뷰 컨트롤러와 데이터 주고받기
  tags:
    - Objective-C
    - iOS
  isStarred: false
  linesHighlighted: []
  key: a94af8b1-869e-4b3d-952d-1b4ee45a7691
  storage: d112f8ec1e85e056a09d
---

iOS에서 다른 뷰 컨트롤러와 데이터 주고받기
---
FirstViewController.m

```objc
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    SecondViewController *viewController = [segue destinationViewController];
    if ([[segue identifier] isEqualToString:@"firstSegue"]) {
        viewController.param = 0;
    } else if ([[segue identifier] isEqualToString:@"secondSegue"]) {
        viewController.param = 1;
    }
}
```

SecondViewController.h

```objc
@interface SecondViewController : UIViewController

@property (assign, nonatomic) int param;

@end
```

---

FirstViewController.m

```objc
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    if ([segue.destinationViewController respondsToSelector:@selector(setParam:)]) {
        [segue.destinationViewController performSelector:@selector(setParam:) withObject:myParam];
    }
}
```