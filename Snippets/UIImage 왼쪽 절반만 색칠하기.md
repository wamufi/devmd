---
title: UIImage 왼쪽 절반만 색칠하기
date: 2024-10-22
time: 20:44:49
tags:
---
```swift
extension UIImage {
    func withLeftHalfOverlayColor(color: UIColor) -> UIImage {
        let rect = CGRect(x: 0, y: 0, width: size.width, height: size.height)
        
        UIGraphicsBeginImageContextWithOptions(size, false, 0.0)
        draw(in: rect)

        let context = UIGraphicsGetCurrentContext()!
        context.setBlendMode(.sourceIn)
        context.setFillColor(color.cgColor)

        let rectToFill = CGRect(x: 0, y: 0, width: size.width * 0.5, height: size.height)
        context.fill(rectToFill)

        let newImage = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()

        return newImage!
    }
}
```

https://stackoverflow.com/questions/22960945/ios-changing-half-of-the-images-color