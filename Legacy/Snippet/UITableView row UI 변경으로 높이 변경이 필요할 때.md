---
boostnote:
  createdAt: '2020-08-20T08:53:34.975Z'
  updatedAt: '2021-12-21T08:55:07.597Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView row UI 변경으로 높이 변경이 필요할 때
  tags:
    - Swift
    - UITableView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: a5629984-08b2-46c1-b93f-90adc85c258a
  storage: d112f8ec1e85e056a09d
---

UITableView row UI 변경으로 높이 변경이 필요할 때
---
[UITableView의 확장과 높이](:note:551f6a6d-e0c2-4f8d-8652-1164b3bda428)
이거보다는 나아보이나 (일단 직접적인 숫자가 안 들어가니까) 오래전에 해서 잘 기억 안 남
뭐 때문에 이거로 처리했는지 기억을 못하고 있다 네

```swift
//UIView.setAnimationsEnabled(false) // beginUpdates시 애니메이션 미적용 애니메이션은 직접 보고 판단

tableView.beginUpdates()
        
if indexPath == expandedIndexPath {
    expandedIndexPath = nil

    UIView.transition(with: view, duration: 0.2, options: .transitionCrossDissolve, animations: { // setAnimationsEnabled(false) 했을 때 애니메이션 적용
        cell.lineView.isHidden = false
        cell.detailView.isHidden = true
        cell.expandedImageView.image = UIImage(named: "ic_down_20_bk")
    }
} else {
    expandedIndexPath = indexPath

    UIView.transition(with: view, duration: 0.2, options: .transitionCrossDissolve, animations: { // setAnimationsEnabled(false) 했을 때 애니메이션 적용
        cell.lineView.isHidden = true
        cell.detailView.isHidden = false
        cell.expandedImageView.image = UIImage(named: "ic_up_20_bk")
    }
}

tableView.layoutIfNeeded() // cell 확장했을 때 짤리는 일이 발생할 때
tableView.endUpdates()
tableView.layoutIfNeeded() // cell이 아주 많을 때 밑에 있는 cell이 안 눌릴 경우
//UIView.setAnimationsEnabled(true) // beginUpdates시 애니메이션 미적용 false 했다면 true로 바꿔주자

```

begin/endUpdates()랑 layoutIfNeeded()가 아마도 핵심
인 줄 알았으나 layoutIfNeeded() 없어도 되더라. 일단 애니메이션 미적용할 때 주석처리하고 처리했음.
+) 걍 때려박음 ㅡㅡ


[How to expand and contract height of a UITableView cell when tapped](https://fluffy.es/how-to-expand-tableview-cell/)
UITextView의 numberOfLines 이용이면 이게 더 나은 것 같기도 함
그러나 나는 텍스트뷰 두 개가 필요했지,,, 그래...

아니면 걍 스택뷰로 히든 처리 하던가
```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    UIView.setAnimationsEnabled(false)
    tableView.beginUpdates()

    let cell = tableView.cellForRow(at: indexPath) as! FriendSettingViewCell
    guard let subviews = cell.indicatorImageView.superview?.superview?.subviews.filter({ $0 != cell.indicatorImageView.superview }) else { return }
    for subview in subviews {
        subview.isHidden.toggle()
    }

    if subviews.last?.isHidden == true {
        cell.indicatorImageView.image = UIImage(named: "ic_down_20_bk")
    } else {
        cell.indicatorImageView.image = UIImage(named: "ic_up_20_bk")
    }

    cell.indicatorImageView.superview?.isHidden = false
//            tableView.reloadRows(at: [indexPath], with: .none)
//            tableView.layoutIfNeeded()
    tableView.endUpdates()
    UIView.setAnimationsEnabled(true)
}
```

+) 1년이 지나고 추가함
상단 코드의 스택뷰 히든 작동은 부드럽지가 않고(애니메이션이 없기 때문), 다른 뷰 선택 시 자동으로 닫히지 않음