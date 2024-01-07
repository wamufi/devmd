---
boostnote:
  createdAt: '2018-11-15T05:18:58.629Z'
  updatedAt: '2020-06-23T06:06:06.007Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UnwindSegue
  tags:
    - Objective-C
    - Swift
    - UX
    - iOS
  isStarred: false
  linesHighlighted: []
  key: 6d4aefe8-74ad-4c25-8e7e-2d4edcf3369b
  storage: d112f8ec1e85e056a09d
---

UnwindSegue
---
RedViewController -> BlueViewController -> GreenViewController

Objective-C

```objc
@implementation RedViewController
- (IBAction)unwindToRed:(UIStoryboardSegue *)unwindSegue {
    // callback 필요할 때
    UIViewController *sourceViewController = unwindSegue.sourceViewController;
    if ([sourceViewController isKindOfClass:[BlueViewController class]]) {
        NSLog(@"BlueViewController to RedViewController");
    } else if ([sourceViewController isKindofClass:[GreenViewController class]]) {
        NSLog(@"GreenViewController to RedViewController");
    }
}

@end
```

Swift

```swift
@IBAction func unwindToMain(_ unwindSegue: UIStoryboardSegue) {
    print("unwind segue: ", unwindSegue)
    print("unwind.source: ", unwindSegue.source)
}
```

[What are Unwind segues for and how do you use them?](https://stackoverflow.com/questions/12561735/what-are-unwind-segues-for-and-how-do-you-use-them)