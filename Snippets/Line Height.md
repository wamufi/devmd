---
title: Line Height
date: 2024-03-04
time: 03:32:47
tags:
  - ios
  - swift
---
```swift
extension UILabel {
    func setTextWithLineHeight(text: String?, lineHeight: CGFloat, alignment: NSTextAlignment? = nil) {
        if let text = text {
            let style = NSMutableParagraphStyle()
            style.maximumLineHeight = lineHeight
            style.minimumLineHeight = lineHeight
            if let alignment = alignment {
                style.alignment = alignment
            }
            
            let offsetDivisor: CGFloat = {
                if #available(iOS 16.4, *) {
                    return 2
                } else {
                    return 4
                }
            }()
            
            let attributes: [NSAttributedString.Key: Any] = [.paragraphStyle: style, .baselineOffset: (lineHeight - font.lineHeight) / offsetDivisor]
            
            let attrString = NSAttributedString(string: text, attributes: attributes)
            self.attributedText = attrString
        }
    }
}
```