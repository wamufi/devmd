---
boostnote:
  createdAt: '2021-07-23T04:46:24.669Z'
  updatedAt: '2021-07-23T04:51:25.557Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIScrollView + Keyboard
  tags:
    - UIScrollView
    - Keyboard
  linesHighlighted: []
  isStarred: false
  key: 7f1f3ffc-2298-4b3a-952a-8ae0581c3237
  storage: d112f8ec1e85e056a09d
---

UIScrollView + Keyboard
---
```swift
override func viewWillAppear() {
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow(_:)), name: UIResponder.keyboardWillShowNotification, object: nil)
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillHide(_:)), name: UIResponder.keyboardWillHideNotification, object: nil)
    
    scrollView.keyboardDismissMode = .interactive
    scrollView.keyboardDismissMode = .onDrag
}

@objc func keyboardWillShow(_ notification: NSNotification) {
    guard let userInfo = notification.userInfo else { return }
    var keyboardFrame = (userInfo[UIResponder.keyboardFrameBeginUserInfoKey] as! NSValue).cgRectValue
    keyboardFrame = self.view.convert(keyboardFrame, from: nil)

    var contentInset = self.scrollView.contentInset
    contentInset.bottom = keyboardFrame.size.height + 20
    scrollView.contentInset = contentInset
}

@objc func keyboardWillHide(_ notification: NSNotification) {
    let contentInset = .zero
    scrollView.contentInset = contentInset
}
```

https://stackoverflow.com/questions/26689232/scrollview-and-keyboard-in-swift
