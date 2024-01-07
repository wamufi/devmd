---
boostnote:
  createdAt: '2021-12-21T08:48:34.607Z'
  updatedAt: '2022-09-13T01:58:44.061Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView 확장과 높이
  tags:
    - iOS
    - Swift
    - UITableView
  linesHighlighted: []
  isStarred: false
  key: c8d48c3b-9d9c-46f6-b86d-e45fb54daac9
  storage: d112f8ec1e85e056a09d
---

UITableView 확장과 높이
---
진짜_진짜_최종.txt
[UITableView의 확장과 높이](:note:551f6a6d-e0c2-4f8d-8652-1164b3bda428)
[UITableView row UI 변경으로 높이 변경이 필요할 때](:note:a5629984-08b2-46c1-b93f-90adc85c258a)

**cell.infoContainerView의 superview는 UIStackView로 안되고 UIView로 생성해야 함**
+) Cell의 setSelected가 유용하게 쓰일 수 있다 

```swift
class ViewController: UIViewController {
    var expandedIndexPath: IndexPath?

    override func viewDidLoad() {
        super.viewDidLoad()
        
//        tableView.estimatedRowHeight = view.frame.width / 4.5 // 접혀져 있을 때의 높이
//        tableView.rowHeight = UITableView.automaticDimension
    }
}

extension ViewController: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
        return view.frame.width / 4.5 // 접혀져 있을 때의 높이
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        if indexPath == expandedIndexPath {
            return UITableView.automaticDimension
        } else {
            if let cell = tableView.cellForRow(at: indexPath) as? CategoryMenuTableViewCell { // 다른 셀 선택 시 접히는 셀 화살표 방향 처리
                cell.arrowImageView.transform = .identity
            }
            return view.frame.width / 4.5 // 접혀져 있을 때의 높이
        }
    }
    
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
//        tableView.deselectRow(at: indexPath, animated: true) // 필요 x
        
//        UIView.setAnimationsEnabled(false)
        tableView.beginUpdates()
        
        let cell = tableView.cellForRow(at: indexPath) as! HistoryTableViewCell
        
        if indexPath == expandedIndexPath { // 펼쳐진 상태에서 클릭 -> 닫힘
            expandedIndexPath = nil
            cell.infoContainerView.isHidden = true
            UIView.animate(withDuration: 0.3) {
                cell.chevronImage.transform = .identify // 화살표 반시계방향 회전 애니메이션 (원래대로)
            }
        } else { // 닫혀진 상태에서 클릭 -> 펼침
            expandedIndexPath = indexPath
            cell.infoContainerView.isHidden = false
            UIView.animate(withDuration: 0.3) {
                cell.chevronImage.transform = CGAffineTransform(rotationAngle: .pi) // 화살표 시계방향 회전 애니메이션
            }
        }
        
//        tableView.layoutIfNeeded() // 필요 x
        tableView.endUpdates()        
//        tableView.layoutIfNeeded() // 필요 x

//        tableView.reloadData() // 애니메이션 필요 없을 때
        
//        UIView.setAnimationsEnabled(true)
    }
    
    func tableView(_ talbeView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "CategoryMenuTableViewCell", for: indexPath) as! CategoryMenuTableViewCell
    
        // 화살표 방향 처리
        if indexPath == expandedIndexPath {
            cell.arrowImageView.transform = CGAffineTransform(rotationAngle: .pi)
        } else {
            cell.arrowImageView.transform = .identity
        }
        
        return cell
    }
}
```

`beginUpdates()`, `endUpdates()` 대신에 `reloadData()` 넣으면 애니메이션 없이 작동