---
boostnote:
  createdAt: '2022-07-12T07:54:42.338Z'
  updatedAt: '2022-09-05T08:56:05.995Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'UIButton의 이미지, 텍스트를 세로 정렬'
  tags: []
  linesHighlighted: []
  isStarred: false
  key: 0f0ae606-8d0e-4f8a-90a4-6ca083b2d1f4
  storage: d112f8ec1e85e056a09d
---

UIButton의 이미지, 텍스트를 세로 정렬
---
```swift
import UIKit

class VerticalImageButton: UIButton {
    
    private var title = ""
    private var icon = UIImage()
    
    // 코드로 생성하기 위해 추가
    required init(title: String, icon: UIImage) {
        self.title = title
        self.icon = icon
        
        super.init(frame: .zero)
        setupView()
    }

    override init(frame: CGRect) {
        super.init(frame: frame)
        
        setupView()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        
        setupView()
    }
    
    // MARK: - UI Setup
    override func prepareForInterfaceBuilder() {
        super.prepareForInterfaceBuilder()
        
        setupView()
    }
    
    private func setupView() {
        snp.makeConstraints { make in
            make.width.equalTo(snp.height).multipliedBy(1.0 / 1.0)
        }
        
        setTitle(title, for: .normal)
        setImage(icon, for: .normal)
                
        titleLabel?.textAlignment = .center
        titleLabel?.font = UIFont.systemFont(ofSize: 12)
        setTitleColor(.black, for: .normal)
    }
    
    override func titleRect(forContentRect contentRect: CGRect) -> CGRect {
        let rect = super.titleRect(forContentRect: contentRect)

        return CGRect(x: 0, y: contentRect.height - rect.height, width: contentRect.width, height: rect.height)
    }

    override func imageRect(forContentRect contentRect: CGRect) -> CGRect {
        let rect = super.imageRect(forContentRect: contentRect)
        let titleRect = self.titleRect(forContentRect: contentRect)

        return CGRect(x: contentRect.width / 2 - rect.width / 2, y: (contentRect.height - titleRect.height) / 2 - rect.height / 2, width: rect.width, height: rect.height)
    }

    override var intrinsicContentSize: CGSize {
        let size = super.intrinsicContentSize

        if let image = imageView?.image {
            var labelHeight: CGFloat = 0
            
            if let size = titleLabel?.sizeThatFits(CGSize(width: self.contentRect(forBounds: self.bounds).width, height: CGFloat.greatestFiniteMagnitude)) {
                labelHeight = size.height
            }

            return CGSize(width: titleLabel!.frame.size.width, height: image.size.height + labelHeight)
        }

        return size
    }

}
```

```swift
let button = VerticalImageButton(title: title!, icon: image)
```