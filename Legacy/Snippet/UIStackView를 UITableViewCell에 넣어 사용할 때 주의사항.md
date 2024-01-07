---
boostnote:
  createdAt: '2021-12-21T08:45:57.661Z'
  updatedAt: '2021-12-21T08:48:19.723Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIStackView를 UITableViewCell에 넣어 사용할 때 주의사항
  tags:
    - UIStackView
    - UITableViewCell
    - iOS
    - AutoLayout
  linesHighlighted: []
  isStarred: false
  key: affecbb3-c2bf-4c07-bf19-7b8bcda84cfa
  storage: d112f8ec1e85e056a09d
---

UIStackView를 UITableViewCell에 넣어 사용할 때 주의사항
---
Cell
ㄴ View
ㄴ StackView
이렇게 사용하였는데 `Unable to simultaneously satisfy constraints. Probably at least one of the constraints in the following list is one you don't want.` 오류가 대폭발

처리 방법은
Storyboard에서 `Bottom Space to: Superview`의 Priority를 999로 설정

놀랍게도 싹 다 없어..졌다..

[ios - Unable to simultaneously satisfy constraints. Probably at least one of the constraints in the following list is one you don't want - Stack Overflow](https://stackoverflow.com/questions/62177264/unable-to-simultaneously-satisfy-constraints-probably-at-least-one-of-the-const)
