---
boostnote:
  createdAt: '2020-09-29T06:17:49.788Z'
  updatedAt: '2020-09-29T06:19:12.366Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: image overlay
  tags:
    - UIImage
    - UIImageView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 3c20c586-e491-458e-b708-720e2024ea2a
  storage: d112f8ec1e85e056a09d
---

image overlay
---
두 장의 이미지를 한 장으로

```swift
imageView.image = thumbnail.overlayWith(image: closeImage, posX: imageView.frame.size.width - closeImage.size.width - 7, posY: 7)
```

```swift
extension UIImage {
    func overlayWith(image: UIImage, posX: CGFloat, posY: CGFloat) -> UIImage {
        let newWidth = size.width < posX + image.size.width ? posX + image.size.width : size.width
        let newHeight = size.height < posY + image.size.height ? posY + image.size.height : size.height
        let newSize = CGSize(width: newWidth, height: newHeight)

        UIGraphicsBeginImageContextWithOptions(newSize, false, 0.0)
        draw(in: CGRect(origin: CGPoint.zero, size: size))
        image.draw(in: CGRect(origin: CGPoint(x: posX, y: posY), size: image.size))
        let newImage = UIGraphicsGetImageFromCurrentImageContext()!
        UIGraphicsEndImageContext()

        return newImage
    }
}
```