---
boostnote:
  createdAt: '2020-10-11T06:41:00.534Z'
  updatedAt: '2020-10-11T06:45:08.008Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableViewCell에서 특정 view hidden
  tags:
    - UITableView
    - UITableViewCell
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 869ce2a2-4d15-4c4e-8df8-5f4f4a54950d
  storage: d112f8ec1e85e056a09d
---

UITableViewCell에서 특정 view hidden
---
이거를 그냥 cellForRow에서 `if indexPath.row == 3 { cell.aView.isHidden = true }` 하면 겹치기 되면서 엉망진창이 된다.
시발 애플 개새끼들 잘 좀 만들지 씹새끼들아 ㅡㅡ

```swift
class MissionRequestTableViewCell: UITableViewCell {
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var descLabel: UILabel!
    @IBOutlet weak var statusLabel: UILabel!
    @IBOutlet weak var refuseButton: UIButton!
    @IBOutlet weak var acceptButton: UIButton!
    @IBOutlet weak var buttonsViewHeight: NSLayoutConstraint!
    @IBOutlet weak var buttonsViewTop: NSLayoutConstraint!
    
    override func awakeFromNib() {
        super.awakeFromNib()
        refuseButton.superview?.isHidden = true
        buttonsViewHeight.constant = 0
        buttonsViewTop.constant = 0
    }
    
    override func prepareForReuse() {
        super.prepareForReuse()
        refuseButton.superview?.isHidden = true
        buttonsViewHeight.constant = 0
        buttonsViewTop.constant = 0
    }
}

extension MissionRequestViewController: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "MissionRequestTableViewCell") as! MissionRequestTableViewCell
        cell.descLabel.text = "IndexPath: \(indexPath)"
        
        let statuses = [2, 5, 7]
        
        for status in statuses {
            if indexPath.row == status {
                cell.refuseButton.superview?.isHidden = false
                cell.buttonsViewHeight.constant = 60
                cell.buttonsViewTop.constant = 20
            }
        }
        
        return cell
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return UITableView.automaticDimension
    }
    
    func tableView(_ tableView: UITableView, estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
        return UITableView.automaticDimension
    }
}
```

여기서는 기본을 true, 특정 row에서 false로 주었다.

`awakeFromNib`은 처음 불렸을 때 처리하는 용도.
`prepareForReuse`는 스크롤 됐을 때 처리하는 용도.
밑에 `heightForRowAt`, `estimatedHeightForRowAt` 두개는 dynamic 용도.