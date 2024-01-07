---
boostnote:
  createdAt: '2019-02-12T08:13:35.133Z'
  updatedAt: '2019-03-06T03:07:49.163Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITextField의 글자수 제한
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 5dab3787-9b06-4d98-b5d4-55b339fcb5d8
  storage: d112f8ec1e85e056a09d
---

UITextField의 글자수 제한
---
```objc
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string {
    int maxLength = 2;
    
    NSString *currentString = textField.text;
    NSString *newString = [currentString stringByReplacingCharactersInRange:range withString:string];
    
    return newString.length <= maxLength;
}
```