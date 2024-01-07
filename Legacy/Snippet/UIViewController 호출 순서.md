---
boostnote:
  createdAt: '2020-09-08T05:18:43.442Z'
  updatedAt: '2020-09-08T05:21:35.027Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIViewController 호출 순서
  tags:
    - Swift
    - UIViewController
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 4b6bac1d-1e50-4481-9393-d33f4391a4cb
  storage: d112f8ec1e85e056a09d
---

UIViewController 호출 순서
---
<h3>A -> B push</h3>
A: viewWillDisappear
B: viewWillAppear isMovingToParent
A: viewDidDisappear
B: viewDidAppear isMovingToParent

<h3>B -> A pop</h3>
B: viewWillDisappear isMovingFromParent
A: viewWillAppear
B: viewDidDisappear isMovingToParent
A: viewDidAppear

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiviewcontroller?changes=__1)