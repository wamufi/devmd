---
boostnote:
  createdAt: '2020-08-11T04:59:04.561Z'
  updatedAt: '2020-08-11T05:04:33.874Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UINavigationBar 폰트 변경
  tags: []
  linesHighlighted: []
  isStarred: false
  key: 0ed528fc-47e2-4e29-a0e9-d4d5ed425438
  storage: d112f8ec1e85e056a09d
---

UINavigationBar 폰트 변경
---
AppDelegate.swift
```swift
UINavigationBar.appearance().tintColor = .black
UINavigationBar.appearance().titleTextAttributes = [NSAttributedString.Key.foregroundColor: UIColor.black, NSAttributedString.Key.font: UIFont.myMediumSystemFont(ofSize: 16)]
UINavigationBar.appearance().largeTitleTextAttributes = [NSAttributedString.Key.foregroundColor: UIColor.black, NSAttributedString.Key.font: UIFont.boldSystemFont(ofSize: 24)]
UIBarButtonItem.appearance().setTitleTextAttributes([NSAttributedString.Key.foregroundColor: UIColor.black, NSAttributedString.Key.font: UIFont.systemFont(ofSize: 16)], for: .normal)
UIFont.preferredFont(forTextStyle: .largeTitle)
```