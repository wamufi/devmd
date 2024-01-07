---
boostnote:
  createdAt: '2022-07-18T06:03:19.477Z'
  updatedAt: '2022-09-05T08:56:01.261Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 2x2 가로 페이징 CollectionView
  tags:
    - Swift
    - iOS
    - UICollectionView
  linesHighlighted: []
  isStarred: false
  key: c65b3e4b-76fb-4d52-a996-7b35c0454b67
  storage: d112f8ec1e85e056a09d
---

2x2 가로 페이징 CollectionView
---
`collectionView.isPagingEnabled = false` 별표 오백개 쳐라 진짜 하
`scrollViewWillEndDragging`으로 페이징 처리 할 때는 paging 켜면 작동 안함 ㅡㅡ

그리고.. 섹션을 활용하자... 2x2 한 칸에 섹션 하나 이렇게
근데 이건 아예 내용이 달라서 가능한 거 일 수도 있다.
카테고리 1에 아이템 4개, 카테고리 2에 아이템 4개, 이런 식이어서.

[[iOS] Swift CollectionViewCell Size 컬렉션뷰 셀 사이즈](https://baechukim.tistory.com/9)
[[iOS] UICollectionView 톺아보기 - 3](https://k-elon.tistory.com/26)
[collectionView paging 해보기!](https://jintaewoo.tistory.com/33)
[[iOS] Swift CollectionViewCell Size 컬렉션뷰 셀 사이즈](https://baechukim.tistory.com/9)

`scrollToItem`이 잘 작동 안할 때는 직접... 만들면 된다...
```swift
/// 카테고리 버튼 선택 시 컬렉션 뷰 이동
@objc func categoryButtonAction(_ sender: UIButton) {
    guard let index = categoryButtons.firstIndex(of: sender) else { return }
    print("!!! index: \(index)")

    let layout = horizontalCollectionView.collectionViewLayout as! UICollectionViewFlowLayout
    let offsetX: CGFloat = (categoryItemSize.width + layout.minimumLineSpacing + layout.sectionInset.left) * CGFloat(index) * categoryNumberOfRows
    print("!!! select offsetX: \(offsetX)")
    horizontalCollectionView.setContentOffset(CGPoint(x: offsetX, y: 0), animated: true)

    setSelectedButton(sender)
}
```
저 `offsetX`는 어디서 나왔냐 하면... 일단 2x2기 때문에 2가 나오고(categoryNumberOfRows) index는 인덱스고 나머지 계산식은 `scrollViewWillEndDragging`의 `cellWidthIncludingSpacing`변수에서 왔다

야쿠르트의 HorizontalPagingView에 삽질의 결과가 나와있음!
사실 hpoint의 제휴사 collectionView와 다를 게 거의 없었다...