---
boostnote:
  createdAt: '2020-08-04T01:34:38.749Z'
  updatedAt: '2020-08-04T01:36:06.802Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Toast
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: b8f4f6b4-39c0-42a9-980c-714e8bd8640c
  storage: d112f8ec1e85e056a09d
---

Toast
---
ToastLabel.swift
```swift
import UIKit

class ToastLabel: UILabel {

    override init(frame: CGRect) {
        super.init(frame: frame)
        
        setupStyle()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        
        setupStyle()
    }
    
    func setupStyle() {
        self.backgroundColor = UIColor(named: "Primary")
        self.textColor = UIColor.white
        self.textAlignment = NSTextAlignment.center;
        self.font = UIFont.systemFont(ofSize: 12)
        self.alpha = 1.0
        self.numberOfLines = 0
        self.lineBreakMode = .byWordWrapping
//        self.clipsToBounds = true
    }
    
    override func drawText(in rect: CGRect) {
        let insets = UIEdgeInsets(top: 15, left: 15, bottom: 15, right: 15)
        super.drawText(in: rect.inset(by: insets))
    }

    override var intrinsicContentSize: CGSize {
        let size = super.intrinsicContentSize
        return CGSize(width: size.width + 15 + 15, height: size.height + 15 + 15)
    }
    
    override var bounds: CGRect {
         didSet {
             preferredMaxLayoutWidth = bounds.width - (15 + 15)
         }
     }
}
```

UIViewController+Extension.swift
```swift
func showToast(message: String) {
//        let alert = UIAlertController(title: nil, message: message, preferredStyle: .alert)
//        alert.view.backgroundColor = UIColor(named: "Primary")
//        alert.view.layer.cornerRadius = 0
//        present(alert, animated: true)
        
    let toastLabel = ToastLabel()
    toastLabel.text = message
    view.addSubview(toastLabel)

    toastLabel.translatesAutoresizingMaskIntoConstraints = false

    let constraints = [toastLabel.centerXAnchor.constraint(equalTo: toastLabel.superview!.centerXAnchor), toastLabel.leadingAnchor.constraint(greaterThanOrEqualTo: toastLabel.superview!.leadingAnchor, constant: 30), toastLabel.trailingAnchor.constraint(lessThanOrEqualTo: toastLabel.superview!.trailingAnchor, constant: -30), toastLabel.bottomAnchor.constraint(equalTo: toastLabel.superview!.bottomAnchor, constant: -40)]
    NSLayoutConstraint.activate(constraints)

    DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + 2.0) {
        UIView.animate(withDuration: 3.5, animations: {
            toastLabel.alpha = 0
        }) { _ in
            toastLabel.removeFromSuperview()
        }
    }
}
```

ViewController.swift
```swift
@IBAction func toastAction(_ sender: Any) {
    showToast(message: "1 test toast 2 test toast 3 test toast 4 test toast 5 test toast 6 test toast")
}
```