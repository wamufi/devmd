---
boostnote:
  createdAt: '2022-11-16T04:42:50.381Z'
  updatedAt: '2022-11-16T04:43:42.106Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: A - B - A일 때 A가 B에서 돌아왔다는 걸 인식
  tags:
    - iOS
    - Swift
  linesHighlighted: []
  isStarred: false
  key: 8e9eb2ba-6087-45e2-8ee1-63e13a2ea822
  storage: d112f8ec1e85e056a09d
---

 A -> B -> A일 때 A가 B에서 돌아왔다는 걸 인식
 ---
 ```swift
 override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)

    // 다른 뷰 컨트롤러로 푸시했다가 돌아올 때
    if !isMovingToParent && !isBeingPresented {
        calcMapViewPadding(toNotchAt: notchIndex)
    }
}
 ```