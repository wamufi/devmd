---
boostnote:
  createdAt: '2020-08-21T05:39:13.118Z'
  updatedAt: '2020-09-01T05:12:14.051Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 'UITableView의 Header, Footer 높이 dynamic'
  tags:
    - Swift
    - UITableView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 67127ba5-a05c-4397-9005-b02c06a4619b
  storage: d112f8ec1e85e056a09d
---

UITableView의 Header, Footer 높이 dynamic
---
headerInSection, footerInSection ㄴㄴ

```swift
override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()

    tableView.sizeHeaderToFit()
    tableView.sizeFooterToFit()
}

extension UITableView {
    func sizeHeaderToFit() {
        guard let headerView = tableHeaderView else { return }
//        let headerView = tableHeaderView!

        headerView.setNeedsLayout()
        headerView.layoutIfNeeded()

        let height = headerView.systemLayoutSizeFitting(UIView.layoutFittingCompressedSize).height
        var frame = headerView.frame
        frame.size.height = height
        headerView.frame = frame

        tableHeaderView = headerView
    }
    
    func sizeFooterToFit() {
        guard let footerView = tableFooterView else { return }
        
        let width = bounds.size.width
        let size = footerView.systemLayoutSizeFitting(CGSize(width: width, height: UIView.layoutFittingCompressedSize.height))
        if footerView.frame.size.height != size.height {
            footerView.frame.size.height = size.height
            tableFooterView = footerView
        }
    }
}
```