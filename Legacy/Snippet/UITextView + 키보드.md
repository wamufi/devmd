---
boostnote:
  createdAt: '2020-06-24T01:36:17.420Z'
  updatedAt: '2020-08-21T00:33:29.457Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITextView + 키보드
  tags:
    - Keyboard
    - UITextView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 41a1c7a8-0a46-4fea-9d70-bddb7efb4b3a
  storage: d112f8ec1e85e056a09d
---

UITextView + 키보드
---
![keyboard_low.gif](dff51ab8.gif)
```swift
//
//  HTalkViewController.swift
//  HPoint2
//

import UIKit

class HTalkViewController: UIViewController {
    
    @IBOutlet var viewModel: TalkViewModel!
    @IBOutlet var tableView: UITableView!
    @IBOutlet var tableHeaderView: UIView!
    @IBOutlet var detailImageView: UIImageView!
    @IBOutlet var replyCountLabel: UILabel!
    @IBOutlet var replyTextView: UITextView!
    @IBOutlet weak var replyViewBottomConstraint: NSLayoutConstraint!
    @IBOutlet var replyViewHeightConstraint: NSLayoutConstraint!
    
    var heightLimit = NSLayoutConstraint()
    var heightLimtConstant: CGFloat = 0.0
    var expandedIndexPath = IndexPath()
    var storyId: String = ""

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.tableView.estimatedRowHeight = 157.5
        self.tableView.rowHeight = UITableView.automaticDimension

        // Do any additional setup after loading the view.
        viewModel.fetchHTalkStory(storyId: storyId) {
            DispatchQueue.main.async {
                self.setupLayout()
                self.tableView.reloadData()
            }
        }
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        becomeFirstResponder()
    }
    
    func setupLayout() {
        detailImageView.setImageFromUrlWithKF(urlString: viewModel.detailImage())
        replyCountLabel.text = "댓글 " + viewModel.getReplyCount() + "개"
        tableView.tableHeaderView?.frame.size.height = adjustHeaderViewHeight() + 53

        registerKeyboardNotifications()
        tableView.keyboardDismissMode = .interactive
        heightLimit = NSLayoutConstraint(item: replyTextView!, attribute: .height, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 0, constant: 0)
        dismissKeyboard()
    }

    func adjustHeaderViewHeight() -> CGFloat {
        let viewWidth = self.view.frame.size.width
//        let imageWidth: CGFloat = 1080.0
//        let imageHeight: CGFloat = 800.0
        let imageWidth = viewModel.detailImageSize().0
        let imageHeight = viewModel.detailImageSize().1
        let ratio = viewWidth / imageWidth
        let scaledHeight = imageHeight * ratio

        return scaledHeight
    }
    
    func dismissKeyboard() {
        let tap = UITapGestureRecognizer(target: self, action: #selector(self.handleDismissKeyboard(_:)))
        tableHeaderView.addGestureRecognizer(tap)
    }
    
    @objc func handleDismissKeyboard(_ sender: UITapGestureRecognizer? = nil) {
        replyTextView.endEditing(true)
    }
    
    func registerKeyboardNotifications() {
        NotificationCenter.default.addObserver(self, selector: #selector(handleKeyboardNotification(_:)), name: UIResponder.keyboardWillShowNotification, object: nil)
        NotificationCenter.default.addObserver(self, selector: #selector(handleKeyboardNotification(_:)), name: UIResponder.keyboardWillHideNotification, object: nil)
    }
    
    @objc func handleKeyboardNotification(_ notification: Notification) {
        if let userInfo = notification.userInfo {
            let keyboardFrame = userInfo[UIWindow.keyboardFrameEndUserInfoKey] as? CGRect
            let isKeyboardShowing = notification.name == UIResponder.keyboardWillShowNotification
            replyViewBottomConstraint.constant = isKeyboardShowing ? +keyboardFrame!.height : 0
            
            UIView.animate(withDuration: 0, delay: 0, options: UIView.AnimationOptions.curveEaseOut, animations: {
                self.view.layoutIfNeeded()
            })
        }
    }
}

extension HTalkViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 10
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "HTalkTableViewCell", for: indexPath) as! HTalkTableViewCell
        
        cell.nameLabel.text = viewModel.getReplyStringAtIndexPath(indexPath).0
        cell.dateLabel.text = viewModel.getReplyStringAtIndexPath(indexPath).1
        cell.messageLabel.text = viewModel.getReplyStringAtIndexPath(indexPath).2
        cell.likeLabel.text = viewModel.getReplyStringAtIndexPath(indexPath).3
        
        return cell
    }
}

extension HTalkViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.beginUpdates()
        
        let cell = tableView.cellForRow(at: indexPath) as! HTalkTableViewCell
        
        if indexPath == expandedIndexPath {
            cell.messageLabel.numberOfLines = 3
            expandedIndexPath = IndexPath()
        } else {
            cell.messageLabel.numberOfLines = 0
            expandedIndexPath = indexPath
        }
        
        replyTextView.endEditing(true)

        tableView.endUpdates()
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return UITableView.automaticDimension
    }
}

extension HTalkViewController: UITextViewDelegate {
    func textViewDidChange(_ textView: UITextView) {
        let numberOfLines = textView.contentSize.height / textView.font!.lineHeight
        
        if 4 < numberOfLines {
            textView.isScrollEnabled = true
            heightLimit.constant = heightLimtConstant + textView.font!.lineHeight
            heightLimit.isActive = true
        } else {
            textView.isScrollEnabled = false
            heightLimtConstant = textView.frame.size.height
            heightLimit.isActive = false
        }
    }
}

```