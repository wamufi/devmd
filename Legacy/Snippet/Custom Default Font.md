---
boostnote:
  createdAt: '2020-07-13T01:21:03.444Z'
  updatedAt: '2020-09-07T02:42:06.634Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Custom Default Font
  tags:
    - Font
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: ce6eeb2b-ab37-4e1a-a21f-c017978ec4c3
  storage: d112f8ec1e85e056a09d
---

Custom Default Font
---
[Set a default font for whole iOS app? - Stack Overflow](https://stackoverflow.com/questions/8707082/set-a-default-font-for-whole-ios-app)
[전체 iOS 앱에 기본 글꼴을 설정 하시겠습니까?](https://lottogame.tistory.com/3985)

Info.plist
```
Fonts provided by Application
- NanumSquareRoundOTFL.otf
- NanumSquareRoundOTFR.otf
- NanumSquareRoundOTFB.otf
- NanumSquareRoundOTFEB.otf
```

UIFont+Extension.swift
```swift
import UIKit

struct AppFontName {
    static let light = "NanumSquareRoundOTFL"
    static let regular = "NanumSquareRoundOTFR"
    static let bold = "NanumSquareRoundOTFB"
    static let heavy = "NanumSquareRoundOTFEB"
}

public extension UIFontDescriptor.AttributeName {
    static let nsctFontUIUsage = UIFontDescriptor.AttributeName(rawValue: "NSCTFontUIUsageAttribute")
}

public extension UIFont {
    static var isOverrided = false
    
    @objc class func mySystemFont(ofSize size: CGFloat) -> UIFont {
        for familyName: String in UIFont.familyNames {
            print("Family Name: \(familyName)")
            for fontName: String in UIFont.fontNames(forFamilyName: familyName) {
                print("--Font Name: \(fontName)")
            }
        }

        return UIFont(name: AppFontName.regular, size: size)!
    }
    
    @objc class func myBoldSystemFont(ofSize size: CGFloat) -> UIFont {
        return UIFont(name: AppFontName.bold, size: size)!
    }
    
    @objc class func myHeavySystemFont(ofSize size: CGFloat) -> UIFont {
        return UIFont(name: AppFontName.heavy, size: size)!
    }
    
    @objc class func myLightSystemFont(ofSize size: CGFloat) -> UIFont {
        return UIFont(name: AppFontName.light, size: size)!
    }
    
    @objc convenience init(myCoder aDecoder: NSCoder) {
        guard let fontDescriptor = aDecoder.decodeObject(forKey: "UIFontDescriptor") as? UIFontDescriptor,
            let fontAttribute = fontDescriptor.fontAttributes[.nsctFontUIUsage] as? String else {
                self.init(myCoder: aDecoder)
                return
            }
        
        var fontName = ""
        // UltraLight, Thin, Light, Regular, Medium, Demi, Bold, Heavy, Black
        // "CTFont***Usage"
        switch fontAttribute {
        case "CTFontRegularUsage":
            fontName = AppFontName.regular
        case "CTFontBoldUsage":
            fontName = AppFontName.bold
        case "CTFontHeavyUsage":
            fontName = AppFontName.heavy
        case "CTFontLightUsage":
            fontName = AppFontName.light
        default:
            fontName = AppFontName.regular
        }
        
        self.init(name: fontName, size: fontDescriptor.pointSize)!
    }
    
    class func overrideInitialize() {
        guard self == UIFont.self, !isOverrided else { return }
        
        isOverrided = true
        
        if let systemFontMethod = class_getClassMethod(self, #selector(systemFont(ofSize:))),
            let mySystemFontMethod = class_getClassMethod(self, #selector(mySystemFont(ofSize:))) {
            method_exchangeImplementations(systemFontMethod, mySystemFontMethod)
        }
        
        if let boldSystemFontMethod = class_getClassMethod(self, #selector(boldSystemFont(ofSize:))),
            let myBoldSystemFontMethod = class_getClassMethod(self, #selector(myBoldSystemFont(ofSize:))) {
            method_exchangeImplementations(boldSystemFontMethod, myBoldSystemFontMethod)
        }
        
        if let initCoderMethod = class_getInstanceMethod(self, #selector(UIFontDescriptor.init(coder:))), let myInitCoderMethod = class_getInstanceMethod(self, #selector(UIFont.init(myCoder:))) {
            method_exchangeImplementations(initCoderMethod, myInitCoderMethod)
        }
    }
}
```

AppDelegate.swift
```swift
//    override init() {
//        super.init()
//        UIFont.overrideInitialize()
//    }

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        UIFont.overrideInitialize()

        return true
    }
```

이렇게 적용할 경우 키보드에서 언어 고를 때의 액션시트 폰트까지 바꿔버린다 시발!
그래서 
```swift
if let systemFontMethod = class_getClassMethod(self, #selector(systemFont(ofSize:))),
            let mySystemFontMethod = class_getClassMethod(self, #selector(mySystemFont(ofSize:))) {
            method_exchangeImplementations(systemFontMethod, mySystemFontMethod)
        }
```
이거를 주석처리 하니까 나오긴 하는데 안 되는게 몇몇개 있다. ex. init 이외 상황 (따로 지정하는 attributed 같은 거) ```UIFont.systemFont(ofSize: 14)```
그런거는 수동으로 바꿔주던지 안되면 걍 주석 해제 해버리던지...
고갱님 태클걸면 그 때 생각하던가 말던가