---
title: "Large Title 사용시 주의사항"
date: 2020-09-08
time: 15:10:59
updatedDate: 2023-07-06
updatedTime: 15:19:49
tags:
  - ios
  - swift
  - navigation
id: 20230706151520
---

Large Title 사용시 주의사항
---
**A -> B에서 A는 large title, B는 아닐 경우의 주의사항**

애니메이션이 작동하면서 부드럽게 돌아가기 위해서는 `viewDidLoad`에 `navigationItem.largeTitleDisplayMode`를 넣어주어야 한다. A에는 always, B에는 never

그리고 `navigationController?.navigationBar.prefersLargeTitle = true`는 수정하면 안된다.
`largeTitleDisplayMode`가 false라고 `prefersLargeTitle`도 false로 줄 경우 부드럽게 작동하지 않는다.
모든 ViewController에 true로 넣어주는게 마음 편함. 아마도... 지금까지는... 그랬다...

[swift - Large title to small title switch in navigation bar is not smooth iOS 13, sticky - Stack Overflow](https://stackoverflow.com/questions/58300644/large-title-to-small-title-switch-in-navigation-bar-is-not-smooth-ios-13-sticky)


**tableView headerInSection과 함께 사용 시 첫 진입 시에 large title이 접혀져서 나오는 경우 (A: never -> B: always)**
`tableView.contentInsetAdjustmentBehavior = .never` 추가
단 tableviewcontroller에서는 작동하지 않는다 ㅅㅂ
tableviewcontroller 사용을 지양하자... 흑흑 static cells 내놔 ㅠㅠ

tableviewcontroller에서는 내렸다가 강제로 내리는 거라 조금 튀긴 하지만
```swift
override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)
    navigationController!.navigationBar.sizeToFit()
}
```
이거라도 쓰자...
+) 시뮬에서는 됐는데 ios 14 실기기에서는 안 된다 아 시발

++) 이게 A -> B 일 때 A가 `largeTitleDisplayMode = .never`였는데         `navigationItem.title = ""` 해주고 `largeTitleDisplayMode = .always`, `navigationController?.navigationBar.prefersLargeTitle = true`로 해주니까 작동 잘 한다 뭐지...? tableviewcontroller에서도 잘 작동한다. 시발???
+++) 안됨 ㅡㅡ 덮어버림 그냥 운이 좋았음,,, 그럼 그렇지 ㅉㅉ

++++)
A viewController
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    navigationItem.largeTitleDisplayMode = .never
}

override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    navigationItem.largeTitleDisplayMode = .never
}

override func viewWillDisappear(_ animated: Bool) {
    super.viewWillDisappear(animated)
    navigationItem.largeTitleDisplayMode = .always
    
//    if isMovingFromParent {
//        navigationItem.largeTitleDisplayMode = .always
//    }
}
```

B viewController
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    navigationItem.largeTitleDisplayMode = .always
}

override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    navigationController!.navigationBar.sizeToFit()
}
```
iOS 14 에서는 되는데 13에서는 안 되는 것 같다 ㅡㅡ 흑흑

+++++) 안 되는 애들 view hierarchy가
```
ViewController
ㄴ View
    ㄴ Safe Area
    ㄴ Table View
```
이딴 식이었는데
```
ViewController
ㄴ View
    ㄴ Safe Area
    ㄴ Scroll View
        ㄴ Table View (IntrinsicTableView)
```
하니까 됐다,,, 애플 시발새끼들아 시발 시발새끼들아 니들만 설정에서 잘 써먹으면 다냐 씹새끼들 ㅡㅡ
TableViewController는 어떻게 하지...?

++++++) 언제까지 늘어날 것인가 라지 타이틀...
절대 절대 절대! scrollView의 Top을 Safe Area에 맞추면 안된다. 무조건 Superview 아무튼 superview 어쨌든 superview.
scrollsToTop을 불렀을 때 superview에 있어야 누구나 생각하는 '그' 동작이 발생한다. 그리고 나는 이것 때문에 머리를 잡았다 시발