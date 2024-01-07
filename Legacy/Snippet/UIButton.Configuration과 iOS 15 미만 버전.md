---
boostnote:
  createdAt: '2021-12-27T08:29:41.674Z'
  updatedAt: '2021-12-27T08:30:57.132Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIButton.Configuration과 iOS 15 미만 버전
  tags:
    - iOS
    - Swift
    - UIButton
  linesHighlighted: []
  isStarred: false
  key: ea96dbf3-bdfb-44c5-a8ca-c664332845d2
  storage: d112f8ec1e85e056a09d
---

UIButton.Configuration과 iOS 15 미만 버전
---
Configuration은 iOS 15 이상부터 사용 가능
근데 두 개 따로 만들기에는 음... 네...
```swift
class HorizontalButton: UIButton {
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        setupLayout()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        
        setupLayout()
    }
    
    override var isSelected: Bool {
        didSet {
            if #available(iOS 15.0, *) {
            } else {
                updateViewForSelected()
            }
        }
    }
    
    // MARK: - 그리기
    private func setupLayout() {
        addTarget(self, action: #selector(tappedButton(_:)), for: .touchUpInside)
        
        if #available(iOS 15.0, *) {
            setupConfiguration()
            setNeedsUpdateConfiguration()
        } else {
            titleLabel?.font = .systemFont(ofSize: 14)
//            titleLabel?.textAlignment = .center
//            titleLabel?.lineBreakMode = .byWordWrapping
            contentEdgeInsets = UIEdgeInsets(top: 0, left: 4, bottom: 0, right: 4)
            
            setTitleColor(UIColor.secondaryLabel, for: .normal)
            setTitleColor(UIColor(named: "AccentColor"), for: .selected)

            updateViewForSelected()
        }
    }
    
    @available(iOS 15.0, *)
    private func setupConfiguration() {
        let title = titleLabel?.text
        
        configurationUpdateHandler = { button in
            var config = button.configuration

            switch button.state {
            case .selected:
                config = .filled()
            case .normal:
                config = .gray()
            default:
                config = .gray()
            }
            
            config?.title = title
            config?.cornerStyle = .medium

            button.configuration = config
        }
    }
    
    private func updateViewForSelected() {
        if isSelected {
            titleLabel?.font = .boldSystemFont(ofSize: 16)
        } else {
            titleLabel?.font = .systemFont(ofSize: 16)
        }
        
        titleLabel?.sizeToFit()
    }
    
    // Touch Event
    @objc func tappedButton(_ sender: UIButton) {
        guard let superview = sender.superview else { return }

        for subview in superview.subviews {
            guard let button = subview as? UIButton else { return }
            button.isSelected = false
        }
        
        isSelected = true
    }
}

```