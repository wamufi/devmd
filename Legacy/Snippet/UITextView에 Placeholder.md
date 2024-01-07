---
boostnote:
  createdAt: '2020-08-21T00:30:53.415Z'
  updatedAt: '2020-08-21T00:32:48.552Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITextView에 Placeholder
  tags:
    - Swift
    - UITextView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 950ad58f-5428-403b-b854-21471ad62346
  storage: d112f8ec1e85e056a09d
---

UITextView에 Placeholder
---

```swift
    var descTextViewPlaceHolder = "문의 사항 입력"
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        descTextView.text = descTextViewPlaceHolder
        descTextView.textColor = UIColor(named: "Grayscale_3")
    }

extension My1vs1QueryViewController: UITextViewDelegate {
    func textViewDidBeginEditing(_ textView: UITextView) {
        descTextWrapperView.borderColor = UIColor(named: "Primary")
        
        if textView.text == descTextViewPlaceHolder {
            textView.textColor = .black
            textView.text = ""
        }
    }
    
    func textViewDidEndEditing(_ textView: UITextView) {
        descTextWrapperView.borderColor = UIColor(named: "Grayscale_5")
        
        if textView.text == "" {
            textView.textColor = UIColor(named: "Grayscale_3")
            textView.text = descTextViewPlaceHolder
        }
    }
}
```