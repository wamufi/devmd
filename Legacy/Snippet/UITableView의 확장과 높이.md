---
boostnote:
  createdAt: '2020-06-16T08:26:27.109Z'
  updatedAt: '2020-08-18T08:09:31.233Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView의 확장과 높이
  tags:
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 551f6a6d-e0c2-4f8d-8652-1164b3bda428
  storage: d112f8ec1e85e056a09d
---

UITableView의 확장과 높이
---
높이는 dynamic하게
확장은 탭하면 길어지게

Unable to simulataneously satisfy constraints 발생 시
view.bottom == superview.bottom의 priority를 750으로 조정하고
클릭 전 보여져야 될 부분의 height constraint를 지정한다. ex. height >= 57

```swift
class ViewController: UIViewController {
    var expandedIndexPath = IndexPath()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
        tableView.estimatedRowHeight = 62
        tableView.rowHeight = UITableView.automaticDimension
    }
}

extension ViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.beginUpdates()
        
        if indexPath == expandedIndexPath {
            expandedIndexPath = IndexPath()
        } else {
            expandedIndexPath = indexPath
        }

        tableView.endUpdates()
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        if indexPath == expandedIndexPath {
            return UITableView.automaticDimension
        } else {
            return 62
        }
    }
}
```

https://stackoverflow.com/questions/33989667/cell-with-2-labels-always-is-unable-to-simultaneously-satisfy-constraints


