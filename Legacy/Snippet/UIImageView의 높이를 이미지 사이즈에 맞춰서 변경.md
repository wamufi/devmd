---
boostnote:
  createdAt: '2020-06-08T03:45:06.827Z'
  updatedAt: '2022-06-17T07:27:02.284Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIImageView의 높이를 이미지 사이즈에 맞춰서 변경
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: bcb2e9d5-88d3-4a1e-9431-a72df8f16126
  storage: d112f8ec1e85e056a09d
---

UIImageView의 높이를 이미지 사이즈에 맞춰서 변경
---
dynamic하게 이미지뷰 사이즈를 변경해보자

Storyboard 사용 시에는 Size Inspect의 **Intrinsic Size를 Placeholder**로 변경해야 한다. 
```swift
import UIKit

class ScaledHeightImageView: UIImageView {
    // Only override draw() if you perform custom drawing.
    // An empty implementation adversely affects performance during animation.
    override func draw(_ rect: CGRect) {
        // Drawing code
        print("draw")
//        contentMode = .scaleAspectFit
    }
    
    override func layoutSubviews() {
        invalidateIntrinsicContentSize() // 스토리보드로 작업 시에 이거 없이 하면 11로 지정해서 스토리보드 작업했을 경우 11 pro에서 실행 시에 사이즈가 다르게 나온다. 시발 이거 때문에 내가 시발 ㅡㅡ
    }
    
    override var intrinsicContentSize: CGSize {
        guard let image = self.image else { return CGSize(width: -1.0, height: -1.0) }
        
        let imageWidth = image.size.width
        let imageHeight = image.size.height
        print("ScaledHeightImageView -- imageWidth: ", imageWidth, " imageHeight: ", imageHeight)
        
        let viewWidth = frame.size.width
        let ratio = viewWidth / imageWidth
        let scaledHeight = imageHeight * ratio
        print("ScaledHeightImageView -- viewWidth: ", viewWidth, " scaledHeight: ", scaledHeight)
        
        return CGSize(width: viewWidth, height: scaledHeight)

        
        // h톡톡 사진 첨부할 때 넣었는데 이벤트 상세보기에서 작동 안함
        /*
        if imageWidth > imageHeight { // 가로
            let viewWidth = self.frame.size.width
            let ratio = viewWidth / imageWidth
            let scaledHeight = imageHeight * ratio
            print("ScaledHeightImageView -- viewWidth: ", viewWidth, " scaledHeight: ", scaledHeight)
            
            return CGSize(width: viewWidth, height: scaledHeight)
        } else { // 세로
            let viewHeight = self.frame.size.height
//            print("viewHeight: ", viewHeight)
            let ratio = viewHeight / imageHeight
            let scaledWidth = imageWidth * ratio
            print("ScaledHeightImageView -- scaledWidth: ", scaledWidth, " viewHeight: ", viewHeight)

            return CGSize(width: scaledWidth, height: viewHeight)
        }
 */
    }
}

```
좆같지만 가로도 쓸모가 있어서 했다.
storyboard에서 imageview의 마진을 정할 일이 생긴다면 priority는 250으로

https://stackoverflow.com/questions/26833627/with-auto-layout-how-do-i-make-a-uiimageviews-size-dynamic-depending-on-the-im

[ios - How to resize UIImageView based on UIImage's size/ratio in Swift 3? - Stack Overflow](https://stackoverflow.com/questions/41154784/how-to-resize-uiimageview-based-on-uiimages-size-ratio-in-swift-3)


+)
[[SWIFT] 테이블 뷰 에서 AutomaticDimension 사용시, 이미지뷰에 따른 높이 조절하기](https://g-y-e-o-m.tistory.com/136?category=237961)
안 써봤지만 흥미로워서 덧붙임

++) 바로 위의 AutomaticDimension 잘 작동함