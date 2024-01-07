---
boostnote:
  createdAt: '2022-09-28T05:52:10.621Z'
  updatedAt: '2022-09-28T07:46:31.264Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIButton의 이미지 위치 변경
  tags:
    - Swift
    - iOS
    - UIButton
  linesHighlighted: []
  isStarred: false
  key: 940b23dc-2a81-42e1-8f45-e6c523128f59
  storage: d112f8ec1e85e056a09d
---

UIButton의 이미지 위치 변경
---
텍스트 왼쪽, 이미지 오른쪽
```swift
func setRightImage() {
    transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
    titleLabel?.transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
    imageView?.transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
}
```

이미지 위쪽, 텍스트 아래쪽
```swift
func setTopImage(padding: CGFloat = 6) {
    guard let imageViewSize = imageView?.frame.size, let titleLabelSize = titleLabel?.frame.size else { return }
    let totalHeight = imageViewSize.height + titleLabelSize.height + padding

    imageEdgeInsets = UIEdgeInsets(top: -(totalHeight - imageViewSize.height), left: 0, bottom: 0, right: -titleLabelSize.width)
    titleEdgeInsets = UIEdgeInsets(top: 0, left: -imageViewSize.width, bottom: -(totalHeight - titleLabelSize.height), right: 0)
    contentEdgeInsets = UIEdgeInsets(top: 0, left: 0, bottom: titleLabelSize.height, right: 0)
}
```

주의사항
`setTopImage()`는 이미지, 텍스트 모두 셋 한 다음에 콜해야 한다... 안 그러면 이미 이전 값에서 사이즈를 불러와 조정을 끝내기 때문에 나중에 텍스트 집어넣고 보면 안 맞음
`setRightImage()`는 확인 안 해봤는데 별 상관 없을 듯

[ios - Label under image in UIButton - Stack Overflow](https://stackoverflow.com/questions/4201959/label-under-image-in-uibutton/59666154#59666154)
