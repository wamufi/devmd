---
boostnote:
  createdAt: '2019-02-21T07:51:29.297Z'
  updatedAt: '2019-03-06T03:07:18.112Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: ViewController에서 replace
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 108b4dc4-0dc2-44ef-8b43-bf80386d374d
  storage: d112f8ec1e85e056a09d
---

ViewController에서 replace
---
A -> B -> C -> "D" -> B (<- A)


```objc
- (void)replaceLoginView {
    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main" bundle:[NSBundle mainBundle]];
    MainViewController *mainViewController = [storyboard instantiateViewControllerWithIdentifier:@"MainViewController"];
    LoginViewController *loginViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginViewController"];
    
    UINavigationController *navigationController = [[UINavigationController alloc] init];
    [navigationController setViewControllers:@[mainViewController, loginViewController] animated:true];
    [self.navigationController showDetailViewController:navigationController sender:nil];
}
```