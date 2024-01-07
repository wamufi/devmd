---
boostnote:
  createdAt: '2020-08-21T00:25:58.916Z'
  updatedAt: '2022-06-30T08:02:41.788Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'UITextView 높이를 입력 받은 만큼, 특정 높이 이하로 늘이기'
  tags:
    - Swift
    - UITextView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: e1584e18-32dc-49e5-aeb9-1ae7f4d0a78a
  storage: d112f8ec1e85e056a09d
---

UITextView 높이를 입력 받은 만큼, 특정 높이 이하로 늘이기
---
![Aug-21-2020 09-35-44.gif](0119071a.gif)

UIView 안에 UITextView를 넣고 margin 20씩 줬음

```swift
    @IBOutlet weak var descTextViewHeight: NSLayoutConstraint! // UITextView 높이
    var descTextViewWrapperMaxHeight: CGFloat = 150 // UIView의 최대 높이

extension ViewController: UITextViewDelegate {
    func textViewDidChange(_ textView: UITextView) {
        if textView.contentSize.height >= descTextViewWrapperMaxHeight - 40 { // == UITextView의 최대 높이
            textView.isScrollEnabled = true
        } else {
            textView.sizeToFit()
            textView.frame.size.height = textView.contentSize.height
            descTextViewHeight.constant = textView.frame.size.height + 40 // UITextView의 높이 + 마진 40
            textView.isScrollEnabled = false
        }
    }
}
```

할 때마다 되니마니 시발


![Aug-25-2020 15-19-58.gif](75a2976d.gif)
```swift
@IBOutlet var replyTextView: UITextView!
@IBOutlet weak var replyViewBottomConstraint: NSLayoutConstraint! // UITextView를 감싸고 있는 UIView의 높이. 여기서는 70

override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    becomeFirstResponder()

    setupTextView()
}

func setupTextView() {
    replyTextView.text = replyTextViewPlaceholder
    replyTextView.textColor = UIColor(named: "Grayscale_3")
    replyTextView.textContainer.lineFragmentPadding = 0
    replyTextView.textContainerInset = .zero
//        replyTextView.frame.size.height = replyTextView.contentSize.height
//        replyViewHeightConstraint.constant = replyTextView.frame.size.height + 34
    replyTextView.sizeToFit()
    replyTextView.frame.size.height = replyTextView.contentSize.height + 48  // textview와 textview를 감싸고 있는 view 사이의 마진은 위 아래 24, 24이다
    replyViewHeightConstraint.constant = replyTextView.frame.size.height
    print("descTextViewHeight: ", replyViewHeightConstraint.constant)
}

func textViewDidChange(_ textView: UITextView) {
    if textView.contentSize.height >= 70 {
        textView.isScrollEnabled = true
    } else {
        textView.isScrollEnabled = false

        let fixedWidth = textView.frame.size.width
        let newSize = textView.sizeThatFits(CGSize(width: fixedWidth, height: CGFloat.greatestFiniteMagnitude))
        textView.frame.size = CGSize(width: max(newSize.width, fixedWidth), height: newSize.height)
        replyViewHeightConstraint.constant = textView.frame.size.height + 48
    }
}

/* 
// 이 코드는 iOS 13에서만 작동했다... 12부터 난리남 ㅡㅡ
func textViewDidChange(_ textView: UITextView) {
    print("contentheight: ", textView.contentSize.height)

    if textView.contentSize.height >= 70 { // 대충 4줄
        textView.isScrollEnabled = true
    } else {
        textView.sizeToFit()
        textView.frame.size.height = textView.contentSize.height + 48 // linespacing은 6
        replyViewHeightConstraint.constant = textView.frame.size.height
        textView.isScrollEnabled = false
    }
}
*/
```

+)
textViewDidChange 없어도 됨... 뭐냐...
GiftPointViewController랑 My1vs1QueryEditViewController랑 참고하삼

++) 220630
GiftPoint는 아예 관련 코드가 없고 My1vs1QueryEdit은 작동되는게 맞긴 함? 확인해보니 작동하는데 아마 저 wrapper 때문인가? 지금 내가 작업하는 거에서 차이는 그것 밖에 없다
My1vs1QueryEdit storyboard에 textview isScrollEnabled false로 되어 있다
그냥 두번째 소스가 제일 나음


[ios - How do I size a UITextView to its content? - Stack Overflow](https://stackoverflow.com/questions/50467/how-do-i-size-a-uitextview-to-its-content)
[sizeToFit / sizeThatFits 함수 알아보기](https://jintaewoo.tistory.com/27)