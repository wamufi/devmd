---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: iOS에서 다른 뷰 컨트롤러와 데이터 주고받기
  tags:
    - Navigation
    - Segue
    - Swift
    - iOS
  isStarred: true
  createdAt: '2017-10-21T02:27:49.878Z'
  updatedAt: '2022-06-07T03:23:07.775Z'
  linesHighlighted: []
  key: 02318177c68f24a76979
  storage: d112f8ec1e85e056a09d
---

iOS에서 다른 뷰 컨트롤러와 데이터 주고받기
---
## 1. 뷰 컨트롤러에 직접 값을 전달하기
1. 값을 받을 VC2를 생성하고 VC1과 연결.
2. VC1에서 VC2에 대한 인스턴스 참조를 얻어오거나, 인스턴스 참조가 없는 경우 인스턴스 생성.
2. 전달하고자 하는 값을 인스턴스의 적절한 프로퍼티에 대입.
3. 화면 전환.

### 1. 프레젠테이션 방식으로 화면 전환하면서 값을 전달하기
```swift
guard let rvc = self.storyboard?.instantiateViewController(withIdentifier: "RVC") as? ResultViewController else {
    return
}

rvc.paramEmail = self.email.text!

self.present(rvc, animated: true)
```

### 2. 내비게이션 컨트롤러를 통해 화면 전환하면서 값을 전달하기
```swift
guard let rvc = self.storyboard?.instantiateViewController(withIdentifier: "RVC") as? ResultViewController else {
    return
}

rvc.paramEmail = self.email.text!

self.navigationController?.pushViewController(rvc, animated: true)
```

### 3. 세그웨이를 이용하여 화면 전환하면서 값을 전달하기
Manual Segue를 생성해 `performSegue(sender: )` 메소드로 Manual Segue 호출.
값 전달은 `prepare(for: sender:)` 메소드에서 처리.

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    let dest = segue.destination
    
    guard let rvc = dest as? ResultViewController else {
        return
    }
    
    rvc.paramEmail = self.email.text!
}
```

```objc
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    if ([segue.destinationViewController respondsToSelector:@selector(setParamEmail:)]) {
        [segue.destinationViewController performSelector:@selector(setParamEmail:) withObject:sender];
    }
}
```
 
## 2. 이전 화면으로 값을 전달하기
다음 화면으로 값을 전달하기와의 차이점
- 인스턴스 생성이 아니라 존재하는 뷰 컨트롤러 인스턴스의 참조값을 얻어와야 함.
- 화면 전환 메소드 대신 화면 복귀 메소드 사용.
- 값을 받은 뷰 컨트롤러가 화면에 값을 표시할 때 인스턴스 초기화가 되지 않으므로 `viewDidLoad()` 대신 `viewWillAppear(_:)` 사용

### 1. 직접 값을 주고 받기 (휘발성)
```swift
let preVC = self.presentingViewController
guard let vc = preVC as? ViewController else {
    return
}

vc.paramEmail = self.email.text

self.presentingViewController?.dismiss(animated: true)
```

### 2. 저장소를 사용하여 값을 주고 받기 (값 저장)
- AppDelegate: 앱이 종료되면 저장된 값도 없어짐
- UserDefaults: 간단한 데이터 저장용
- Core data: 소규모 데이터베이스 같은 데이터 저장용
- ex: 파일에 저장, 서버에 데이터 전송해서 저장 등

#### 1. AppDelegate 사용
AppDelegate는 앱 전체를 통틀어 하나의 인스턴스만 존재하도록 보장됨 = Singleton
직접 생성은 불가능하고 현재 생성되어 있는 인스턴스를 참조해야 함.

VC2 예시 코드

```swift
let ad = UIApplication.shared.delegate as? AppDelegate

ad?.paramEmail = self.email.text

self.presentingViewController?.dismiss(animated: true)
```

VC1 예시 코드
```swift
let ad = UIApplication.shared.delegate as? AppDelegate

if let email = ad?.paramEmail {
    resultEmail.text = email
}
```

#### 2. UserDefaults 사용
앱이 삭제되기 전까지는 저장된 값 유지

VC2 예시 코드

```swift
let ud = UserDefaults.standard

ud.set(self.email.text, forKey: "email")

self.presentingViewController?.dismiss(animated: true)
```

VC1 예시 코드

```swift
let ud = UserDefaults.standard

if let email = ud.string(forKey: "email") {
    resultEmail.text = email
}
```

### 3. Delegate 사용
VC1 예시 코드
```swift
class FirstViewController: UIViewController {
    var delegate: FirstViewControllerDelegate?
    
    func getVC2Param {
        resultEmail.text = delegate?.getResultEmail()
    }
}

protocol ViewControllerDelegate {
    func getResultEmail() -> String
}
```

VC2 예시 코드
```swift
class SecondViewController: UIViewController {
    func setVC2Param {
        if let firstVC = presentingViewController as? ViewController {
            firstVC.delegate = self
        }
    }
}

extension SecondViewController: FirstViewControllerDelegate {
    func getResultEmail() -> String {
        return resultEmail
    }
}
```