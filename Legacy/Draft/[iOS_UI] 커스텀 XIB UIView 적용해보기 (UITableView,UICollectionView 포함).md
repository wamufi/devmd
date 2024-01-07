---
boostnote:
  createdAt: '2022-07-11T04:02:24.443Z'
  updatedAt: '2022-07-22T07:30:27.396Z'
  type: MARKDOWN_NOTE
  folder: 69f287614baf36ef63db
  title: '[iOS/UI] 커스텀 XIB UIView 적용해보기 (UITableView,UICollectionView 포함)'
  tags: []
  linesHighlighted: []
  isStarred: false
  key: 591a8b76-81ee-43a8-88d6-e762a794a00d
  storage: d112f8ec1e85e056a09d
---

[[iOS/UI] 커스텀 XIB UIView 적용해보기 (UITableView,UICollectionView 포함)](https://fomaios.tistory.com/entry/iOS-%EC%BB%A4%EC%8A%A4%ED%85%80-XIB-UIView-%EC%A0%81%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0-Custom-XIB-UIView)
[XIB를 사용한 UIView Custom 제대로 이해하기. #iOS #XIB #UIView #Custom \| by whitelips | a day of a programmer | Medium](https://medium.com/a-day-of-a-programmer/xib%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-uiview-custom-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-348a9b789496)
두번째 글이 xib와 init을 동시에 사용하는 법을 알려준다 별표 5백개는 치는 건 아니고 아무튼 그렇다

[iOS) 커스텀 뷰 Xib 연결하기](https://gyuios.tistory.com/125)
[[Swift] Custom View 만들기](https://nsios.tistory.com/83)
[ios - EXC_BAD_ACCESS on custom UIView with custom XIB - Stack Overflow](https://stackoverflow.com/questions/19355104/exc-bad-access-on-custom-uiview-with-custom-xib)
[[Swift] xib 파일로 View 관리하기](https://jintaewoo.tistory.com/55)

xib로 했을 때 layoutSubviews()는 여러번 콜 하니까 awakeFromNib()에 setupUI()를 넣으면 잘 작동한다