---
boostnote:
  createdAt: '2022-07-14T07:02:16.138Z'
  updatedAt: '2022-08-12T01:10:15.742Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UICollectionView height dynamically in UIView
  tags:
    - Swift
    - iOS
    - UICollectionView
  linesHighlighted: []
  isStarred: false
  key: df20049f-680e-410f-8b5c-90c903459b5a
  storage: d112f8ec1e85e056a09d
---

UICollectionView height dynamically in UIView
---
구조는
UIViewController
ㄴ UIView
   ㄴ UIScrollView
      ㄴ UIStackView
         ㄴ UIView
            ㄴ UICollectionView
            
와ㅡ우
야쿠르트의 `HorizontalScrollView`, `HorizontalPagingView` 참고

IntrinsicCollectionView를 하면 `cellForItemAt`을 아예 콜을 안 함
당연히 `collectionView.intrinsicContentSize.height` 작동 안 함
`collectionViewHeight.constant = collectionView.collectionViewLayout.collectionViewContentSize.height` 작동 안 함

하

UICollectionView가 있는 UIView에서 UICollectionView의 높이를 Constraint로 잡은 후 `draw(_ rect: CGRect)`에서 `collectionViewHeight.constant = itemSize.height` 하면 끗

itemSize는 그냥 말 그대로 `CGSize(width: 300, height: 400)`이다

`layoutSubviews()`에 안 넣은 이유는 콜을 자주 해서...
아 그리고 `flowLayout.itemSize`는 꼭 넣어야 함 안 그러면 `cellForItemAt` 콜 안 한다.

+) IntrinsicCollectionView는 가로 스크롤일 때는 작동 안 했고 세로로 그냥 뿌리는 건 작동함 ㅡㅡ