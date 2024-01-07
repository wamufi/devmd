---
boostnote:
  createdAt: '2019-02-10T08:40:46.218Z'
  updatedAt: '2019-03-06T03:07:59.162Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIScrollView + UITextField
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 53e70425-2f9e-4f99-8cc1-ddc40bb60e43
  storage: d112f8ec1e85e056a09d
---

UIScrollView + UITextField
---
키보드 문제를 해결해보자 시발
[UIScrollView - Move hidden TextFields with keyboard show and hide
](https://www.youtube.com/watch?v=w9uVsvL7eyM)
[[Text Programming Guide for iOS 번역] 스크롤 뷰에서 키보드 다루기](https://oaksong.github.io/2018/04/14/text-programming-guide-managing-the-keyboard/)

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    
    UITapGestureRecognizer *tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(didTapView)];
    [self.view addGestureRecognizer:tapGesture];
}

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    
    [self registerForKeyboardNotifications];
}

- (void)viewWillDisappear:(Bool)animated {
    [super viewWillDisappear:animated];
    
    [self unregisterForKeyboardNotifications];
}

- (void)didTapView {
    [self.view endEditing:true];
}

- (void)registerForKeyboardNotifications {
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillShow:) name:UIKeyboardWillShowNotification object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillBeHidden:) name:UIKeyboardWillHideNotification object:nil];
}

- (void)unregisterForKeyboardNotifications {
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}

- (void)keyboardWillShow:(NSNotification*)notification {
    NSDictionary* info = [notification userInfo];
    CGSize kbSize = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue].size;
    
    UIEdgeInsets contentInsets = UIEdgeInsetsMake(0.0, 0.0, kbSize.height, 0.0);
    _scrollView.contentInset = contentInsets;
}

- (void)keyboardWillBeHidden:(NSNotification*)notification {
    UIEdgeInsets contentInsets = UIEdgeInsetsZero;
    _scrollView.contentInset = contentInsets;
//    _scrollView.scrollIndicatorInsets = contentInsets;
}
```