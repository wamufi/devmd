---
boostnote:
  createdAt: '2020-07-15T06:40:31.839Z'
  updatedAt: '2020-07-15T06:41:45.383Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIActivityIndicatorView
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: eae907cf-95aa-448e-ab65-87363b31b83c
  storage: d112f8ec1e85e056a09d
---

UIActivityIndicatorView
---
UIViewController+Extension.swift
하나 만들고 계속 써보자 어예

```swift
extension UIViewController {
    private var indicatorViewTag: Int {
        return 0715
    }
    
    // MARK: - Activity Indicator View
    func showActivityIndicator() {
        let indicatorView = UIActivityIndicatorView()
        if #available(iOS 13.0, *) {
            indicatorView.style = .large
        } else {
            indicatorView.style = .whiteLarge
        }
        indicatorView.tag = indicatorViewTag
        
        indicatorView.center = view.center
        indicatorView.hidesWhenStopped = true
                
        DispatchQueue.main.async {
            indicatorView.startAnimating()
            self.view.addSubview(indicatorView)
        }
    }
    
    func dismissActivityIndicator() {
        if let indicatorView = view.subviews.filter({ $0.tag == indicatorViewTag }).first as? UIActivityIndicatorView {
            indicatorView.stopAnimating()
            indicatorView.removeFromSuperview()
        }
    }
}
```