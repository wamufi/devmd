---
boostnote:
  createdAt: '2022-07-05T04:39:44.474Z'
  updatedAt: '2022-07-05T04:53:08.968Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'UIScrollView scrollViewDidScroll(_ scrollView: UIScrollView)'
  tags:
    - Swift
    - iOS
    - UIScrollView
  linesHighlighted: []
  isStarred: false
  key: f5807534-7f5f-4fac-b778-f0cfee6c74fc
  storage: d112f8ec1e85e056a09d
---

UIScrollView scrollViewDidScroll(_ scrollView: UIScrollView)
---
여기서 써먹을만한 잡다한 것들

```swift
func scrollViewDidScroll(_ scrollView: UIScrollView) {
    // 세로 움직임 캐치
    let translation = scrollView.panGestureRecognizer.translation(in: scrollView.superview)
    if translation.y > 0 { // 위에서 아래로

    } else { // 아래에서 위로

    }
    
    // 현재 y값
    let y = scrollView.contentOffset.y
}
```

https://stackoverflow.com/questions/31857333/how-to-get-uiscrollview-vertical-direction-in-swift
