---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: iOS의 화면 전환 방법
  tags:
    - Navigation
    - Segue
    - Swift
    - iOS
  isStarred: true
  createdAt: '2017-10-15T13:00:54.006Z'
  updatedAt: '2022-06-07T03:23:14.839Z'
  linesHighlighted: []
  key: 0e6926d570578c617745
  storage: d112f8ec1e85e056a09d
---

iOS의 화면 전환 방법
---
1. 뷰 컨트롤러의 뷰 위에 다른 뷰를 가져와 바꿔치기 하기
2. 뷰 컨트롤러에서 다른 뷰 컨트롤러를 호출하여 화면 전환하기
3. 내비게이션 컨트롤러를 사용하여 화면 전환하기
4. 화면 전환용 객체 세그웨이(Segueway)를 사용하여 화면 전환하기

화면 전환이 가지고 있는 특성
---
- 다음 화면으로 이동하는 방법과 이전 화면으로 되돌아가는 방법이 다름
  - 다음 화면은 +1, 이전 화면은 -1
- 화면 전환 방식에 따라 이전 화면으로 되돌아가는 방법이 다름

### 1. 뷰를 이용한 화면 전환
뷰 컨트롤러 내에서 뷰를 바꿔치기
ex. AutoLink의 Find Dealer에서 지도/목록 화면
    하나의 뷰 컨트롤러에서 지도/목록 뷰만 바꿔치기 했다.

### 2. 뷰 컨트롤러 직접 호출에 의한 화면 전환
VC1에서 presentedViewController 속성으로 VC2 참조, VC2에서 presentingViewController 속성으로 VC1 참조
> A presentViewController B -> B presentViewController C 일 경우 
> B 기준으로 A는 presentingViewController, C는 presentedViewController

VC1 예시코드

```swift
// 이동할 뷰 컨트롤러 객체를 StoryboardID 정보를 이용해 참조
let uvc = self.storyboard!.instantiateViewController(withIdentifier: "SecondVC")
self.present(uvc, animated: true)
```

VC2 예시코드

```swift
self.presentingViewController?.dismiss(animated: true)
```

### 3. 내비게이션 컨트롤러를 이용한 화면 전환
내비게이션에서는 뒤로 가기 버튼을 직접 만들 필요가 없음. 구현도 가능함.
VC1 예시코드

```swift
// 이동할 뷰 컨트롤러 객체를 StoryboardID 정보를 이용해 참조
guard let uvc = self.storyboard!.instantiateViewController(withIdentifier: "SecondVC") else {
    return
}
self.navigationController?.pushViewController(uvc, animated: true)
```

VC2 예시코드

```swift
_ = self.navigationController?.popViewController(animated: true)
```

### 4. 세그웨이를 이용한 화면 전환

#### 1. Action Segue
컨트롤이 출발점. 이벤트 트리거에 자동으로 연결
- Present Mdoally 타입: `present(_: animated:)`
- Show 타입: Navigation Controller (단, 내비게이션 컨트롤러가 존재할 경우에만)

#### 2. Manual Segue
뷰 컨트롤러가 출발점. `performSegue(withIdentitifier: sender:)`

```swift
self.performSegue(withIdentitifer: "Identitifier", sender: self)
```

#### 3. Unwind - 화면 복귀
- presentation: `dismiss(animated:)`
- navigation controller: `popViewController(animated:)`

##### 1. Unwind Segue
1. VC2에서 VC1로 (VC1 예시코드)

```swift
@IBAction func unwindToVC1(_ segue: UIStoryboardSegue) { }
```

2. VC4에서 VC1로 (VC1 예시코드)

```swift
@IBAction func unwindToVC1(_ sender: UIStoryboardSegue) { }
```

3. VC4에서 VC2로 (VC2 예시코드)

```swift
@IBAction func unwindToVC2(_ segue: UIStoryboardSegue) { }
```

#### 4. 커스텀 세그
Action Segue 타입 중에서 Custom 선택
다른 컨트롤러에 있는 뷰만 읽어와 전환하는 세그웨이 예시 코드

```swift
class CustomSegue: UIStoryboardSegue {
    override func perform() {
        // 세그웨이의 출발지 뷰 컨트롤러
        let srcUVC = self.source
        // 세그웨이의 목적지 뷰 컨트롤러
        let destUVC = self.destination
        
        // fromView에서 toView로 뷰 전환
        UIView.transition(from: srcUVC.view, to: destUVC.view, duration: 2, options: .transitionCurlDown)
    }
}
```

##### 1. 전처리 메소드 prepare
`prepare(for segue: UIStoryboardSegue, sender: Any?) { }`

예시 코드

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if (segue.identifier == "segue_one") {
        NSLog("segue_one")
    } else if (segue.identifier == "segue_two") {
        NSLog("segue_two")
    } else {
        NSLog("else segue")
    }
}
```