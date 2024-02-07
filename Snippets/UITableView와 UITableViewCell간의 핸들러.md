---
title: UITableView와 UITableViewCell간의 핸들러
date: 2024-02-07
time: 10:35:56
tags: 
- ios
- swift
- uitableview
- uitableviewcell
---
`tableView(_:didSelectRowAt:)`랑 `setSelected`를 사용하면 편리하지만 그런 경우가 아닐 경우

UITableViewCell.swift
```swift
class Cell: UITableViewCell {

	private var etcHandler: (() -> Void)? = nil

	func configure(with item: Item?) {
		let checkbox = Checkbox()
        checkbox.setTitle("기타", for: .normal)
        checkbox.addTarget(self, action: #selector(etcAction(_:)), for: .touchUpInside)
        stackView.addArrangedSubview(checkbox)
	}
	
	@objc private func etcAction(_ sender: UIButton) {
        etcTextView.isHidden.toggle()
        hasCheckedEtc.toggle()
        
        etcTextView.superview?.sizeToFit()
        etcTextView.superview?.layoutIfNeeded()
        os_log("etc action sender: %@", "\(etcTextView.isHidden)")
        
        etcHandler?()
    }
    
    func onEtcHandler(_ handler: @escaping () -> Void) {
        etcHandler = handler
    }
}
```

UIViewController.swift
```swift
class ViewController: UIViewConroller {

	func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
		let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath) as! Cell
	
		cell.configure(with: item)
		cell.onEtcHandler {
			// UIView.performWithoutAnimation {
			tableView.beginUpdates()
			tableView.endUpdates()
			// }
		}
	
		return cell
	}
}
```

https://medium.com/@pichratanak/build-a-self-sizing-uitableviewcell-with-uistackview-7daa2bd85630

delegate 사용은
https://medium.com/grand-parade/animating-uitableviewcell-size-and-text-change-using-auto-layout-and-automatic-dimension-a64bf8cb01b2

참고용 기존 링크들
[[UITableView 확장과 높이]]
[[UITableView의 확장과 높이]]
