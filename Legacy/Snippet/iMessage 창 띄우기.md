---
boostnote:
  createdAt: '2020-07-13T06:53:26.495Z'
  updatedAt: '2020-07-17T01:04:30.956Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: iMessage 창 띄우기
  tags:
    - Message
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: dcc58dd6-f1bd-4c5a-985f-605db7f3809c
  storage: d112f8ec1e85e056a09d
---

iMessage 창 띄우기
---
```swift
import MessageUI

class ViewController: UIViewController {
    @IBAction func smsAction(_ sender: Any) {
        let regex = "^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$"

        if phoneNumberField.text?.range(of: regex, options: .regularExpression) != nil {
            let inviteMessage = "* \(custName) 님으로 부터 H.Point 가입 초대장이 도착하였습니다.\n현대백화점 그룹의 H.Point에 가입하시면,\n가입 당월 최대 5,000 포인트 적립 등 많은 헤택이 기다리고 있습니다.\n\n추천인 코드 : \(recommandNo) \n회원가입 링크 \(INVITE_JOIN_URL)"
            
            if MFMessageComposeViewController.canSendText() {
                let composeViewController = MFMessageComposeViewController()
                composeViewController.body = inviteMessage
                composeViewController.recipients = [phoneNumberField.text!]
                composeViewController.messageComposeDelegate = self
                self.present(composeViewController, animated: true, completion: nil)
            }
        } else {
            showAlert(alertTitle: "title", alertMsg: "phone number wrong")
        }
    }
}

extension FriendInviteViewController: MFMessageComposeViewControllerDelegate {
    func messageComposeViewController(_ controller: MFMessageComposeViewController, didFinishWith result: MessageComposeResult) {
        controller.dismiss(animated: true, completion: nil)
    }
}

```