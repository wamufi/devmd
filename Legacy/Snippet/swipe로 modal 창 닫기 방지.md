
---
title: "swipe로 modal 창 닫기 방지"
date: 2022-05-11
time: 15:08:38
updatedDate: 2023-07-11
updatedTime: 16:06:33
tags:
  - ios
  - swift
  - UIAdaptivePresentationControllerDelegate
id: 20230711160616
---

swipe로 modal 창 닫기 방지
---
![May-11-2022 15-15-40.gif](08ef10cd.gif)

`isModalInPresentation`은 UIViewController의 Lifecycle 관련 함수에서만 작동되는 것 같다. 아마도? 다른 함수에서는 바뀌지가 않았다. 그래서 `presentationControllerShouldDismiss` 사용

delegate 설정
네비게이션이 없을 경우에는 navigationController 부분 삭제
+) 네비게이션이 없어도 navigationController 있어야 되는데? 없으니까 delegate 안 탐
```swift
navigationController?.presentationController?.delegate = self
```

delegate 구현
```swift
extension CourseCreateViewController: UIAdaptivePresentationControllerDelegate {
    // isModalInPresentation, presentationControllerShouldDismiss가 false일 경우에만 부른다
    func presentationControllerDidAttemptToDismiss(_ presentationController: UIPresentationController) {
        let alert = UIAlertController(title: nil, message: "선택한 장소를 추가하시겠습니까?", preferredStyle: .alert)
        let addAction = UIAlertAction(title: "선택한 장소 추가", style: .default) { _ in
//                resultsController.dismiss(animated: true)
        }
        let discardAction = UIAlertAction(title: "선택한 장소 폐기", style: .default) { _ in
            // 검색 결과 창만 닫힘
            self.searchController.searchResultsController?.dismiss(animated: true)
        }

        alert.addAction(addAction)
        alert.addAction(discardAction)
        alert.preferredAction = addAction

        self.present(alert, animated: true)
    }
    
    // isModalInPresentation과 같은 동작을 하는 함수
    // true일 경우 창이 닫히고, false일 경우 닫히지 않는다
    func presentationControllerShouldDismiss(_ presentationController: UIPresentationController) -> Bool {
        if selectedRowCount > 0 {
            return false
        } else {
            return true
        }
    }
}
```