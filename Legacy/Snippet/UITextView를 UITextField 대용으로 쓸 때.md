---
boostnote:
  createdAt: '2018-03-11T09:50:13.186Z'
  updatedAt: '2019-03-06T03:09:07.529Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITextView를 UITextField 대용으로 쓸 때
  tags:
    - Objective-C
    - UITextView
    - iOS
  isStarred: false
  linesHighlighted: []
  key: 8d4689de353c4832f955
  storage: d112f8ec1e85e056a09d
---

UITextView를 UITextField 대용으로 쓸 때
---
1. 키보드 활성화에 따라 view 올리고 내리고 하기
2. Return 키로 키보드 내리기


```objc
#pragma mark - UITextView Delegate

- (BOOL)textView:(UITextView *)textView shouldChangeTextInRange:(NSRange)range replacementText:(NSString *)text {
    if ([text isEqualToString:@"\n"]) {
        [textView resignFirstResponder];
        return false;
    }
    
    return true;
}

- (void)textViewDidBeginEditing:(UITextView *)textView {
    [self animateTextView:true];
}

- (void)textViewDidEndEditing:(UITextView *)textView {
    [self animateTextView:false];
}

- (void)animateTextView:(BOOL)isUp {
    const int movementDistance = 160.0f;
    const float movementDuration = 0.3f;
    int movement = (isUp ? -movementDistance : 0);
    
    [UIView beginAnimations:@"anim" context: nil];
    [UIView setAnimationBeginsFromCurrentState:true];
    [UIView setAnimationDuration: movementDuration];
    self.view.frame = CGRectOffset(_surveyView.frame, 0, movement);
    [UIView commitAnimations];
}
```
