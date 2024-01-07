---
boostnote:
  createdAt: '2020-09-01T08:03:12.481Z'
  updatedAt: '2020-09-16T02:12:13.768Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIScrollView 안에서 상단 뷰 띄우기
  tags:
    - Swift
    - UIScrollView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: afffdb3b-df89-46c5-95c7-f09b39d76a9f
  storage: d112f8ec1e85e056a09d
---

UIScrollView 안에서 상단 뷰 띄우기
---
navigation large title이랑 같이 쓰려고 하다 보니 고충이 이만저만이 아니었다.
large title을 안 쓸 경우에는 그냥 safe area 밑에 view, scrollview 만들어서 쓰면 그만이다. 그러나 **large title은 scrollview랑만 적용이 되다보니(스크롤이)** 저렇게 만들 경우에는 이미 scroll top에서 밑으로 당겼을 경우 고정 뷰는 가만히 있고 title과 scorllview만 움직여서 아주 못 볼 꼴이 완성된다.

![Sep-01-2020 17-07-13.gif](ac24dca9.gif)

이 gif에서 '전환'은 large title, 'H.point로 전환'은 UIScrollView 안의 FixedView, 그 외에는 UIScrollView 안의 View (Scroll)

**스토리보드 구조**
```
Safe Area
UIScrollView
    ㄴ Content Layout Guide
    ㄴ Frame Layout Guide
    ㄴ View (Scroll)
    ㄴ FixedView
```

```swift
override func viewDidLoad() {
    fixedViewFrameConstraint.isActive = false
}

extension HPointChangeViewController: UIScrollViewDelegate {
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        if scrollView.contentOffset.y > 0 {
            fixedViewFrameConstraint.isActive = true
            fixedViewContentConstraint.isActive = false
        } else {
            fixedViewFrameConstraint.isActive = false
            fixedViewContentConstraint.isActive = true
        }
    }
}
```


View (Scroll)은 top을 Content + 60
FixedView는 Frame == top, Content == top

스크롤이 0이고 내려갈 때는 Content 적용, 그 외에는 Frame 적용
끗

+) ScrollView안에 TableView를 넣어야 될 경우 IntrinsicTableView를 적용해야 한다. 안 그러면 작동 안 함.

자다가 이거 때문에 깨기도 했는데 시발 ㅡㅡ

[Floating button 만들기 in ScrollView :: 삼쓰의 개발 블로그](https://woongsios.tistory.com/58)

여기서 Frame은 스크롤을 안 하는 가이드라고 해서 이거 보고 적용함 ㄱㅅㄱㅅ


++) 그냥 large title은 headerview로 넣고 탭은 section header로 넣어도 됐을 것 같다 시발... ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ 아 미안해요