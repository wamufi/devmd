---
boostnote:
  createdAt: '2022-07-13T08:33:25.250Z'
  updatedAt: '2022-07-14T01:50:55.593Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: StckView에서 특정 subview에서만 마진 주기
  tags:
    - Swift
    - iOS
    - UIStackView
  linesHighlighted: []
  isStarred: false
  key: e4ca4676-0866-4cfa-b147-5a391b5a450f
  storage: d112f8ec1e85e056a09d
---

StckView에서 특정 subview에서만 마진 주기
---
 extension
 ```swift
 extension UIStackView {
     func addArrangedSubview(_ view: UIView, edgeInsets insets: UIEdgeInsets) {
        let containerForMargin = UIView()
        containerForMargin.addSubview(view)
        view.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            view.topAnchor.constraint(equalTo: containerForMargin.topAnchor, constant:insets.top),
            view.bottomAnchor.constraint(equalTo: containerForMargin.bottomAnchor, constant: -insets.bottom),
            view.leftAnchor.constraint(equalTo: containerForMargin.leftAnchor, constant: insets.left),
            view.rightAnchor.constraint(equalTo: containerForMargin.rightAnchor, constant: insets.right)
        ])

        addArrangedSubview(containerForMargin)
    }
  }
 ```
 
 사용
 ```swift
stackView.addArrangedSubview(todayView, edgeInsets: UIEdgeInsets(top: 0, left: 0, bottom: 40, right: 0))
 ```
 
 bottom이 -인 이유는 보통 좁히려고는 안 하기 때문에..? 나는 그렇다