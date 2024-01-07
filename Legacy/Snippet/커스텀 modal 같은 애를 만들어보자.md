---
boostnote:
  createdAt: '2020-09-18T02:00:50.197Z'
  updatedAt: '2022-06-08T08:29:25.832Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 커스텀 modal 같은 애를 만들어보자
  tags:
    - Popup
    - Swift
    - UIPresentationController
    - iOS
  linesHighlighted: []
  isStarred: false
  key: c917d20d-1a6b-4eff-b0c4-b9a0f21ebcda
  storage: d112f8ec1e85e056a09d
---

커스텀 modal 같은 애를 만들어보자
---
hpoint에서 FilterPresentationController 검색하면 많이 나온다
제스처도 추가함 아니제스처를넣어달라고했으면영역을똑바러잡던가
그리고 .bottom, .right도 추가함 하다가거품물었다 right는 그냥 라이브러리 쓰자 ㅡㅡ

bottom 기본 높이는 view에서 statusbar랑 navigationbar 높이 뺀 높이.
right 기본 너비는 view에서 -30

뷰 사이즈에 따른 자유 높이는 AlphabetSelectViewController에서 만들었는데
그냥 적당한 사이즈의 뷰를 하나 만들고 기본 뷰는 clear 처리했다.
그러니까 FilterPresentationController위에 ViewController가 올라가는 건 같은데 AlphabetSelectViewController는 그 위의 뷰 높이를 조정하는 fake를 침
Alphabet.storyboard랑 거기 확인하세요 제가 지금 바빠서요

[half-modal 하프모달 뷰 컨트롤러 만들기](https://iamcho2.github.io/2021/06/25/half-modal-viewcontoller)[[iOS] UIPanGestureRecognizer 사용해서 모달화면 아래로 dismiss하기 :: 후리스콜링개발](https://roniruny.tistory.com/135)
[iOS) Presentation 과 Transition 그리고 Animation... (2) :: 커다란 Scuti UY](https://koggy.tistory.com/4)

+)
뒤에 있는 view controller 터치가 안된다. [GitHub - applidium/OverlayContainer: Non-intrusive iOS UI library to implement overlay based interfaces](https://github.com/applidium/OverlayContainer) 이거 써라 그냥


```swift
//
//  FilterPresentationController.swift
//  HPoint2
//

import UIKit

class FilterPresentationController: UIPresentationController {
    
    private let direction: PresentationDirection
    
    let blurEffectView: UIVisualEffectView!
    var tapGestureRecognizer : UITapGestureRecognizer = UITapGestureRecognizer()
    var statusBarHeight: CGFloat {
         let statusBarSize = UIApplication.shared.statusBarFrame.size
         return Swift.min(statusBarSize.width, statusBarSize.height)
     }
    var navigationBarHeight: CGFloat = 0
    
    init(presentedViewController: UIViewController, presenting presentingViewController: UIViewController?, direction: PresentationDirection = .bottom) {
        self.direction = direction
        
        var blurEffect = UIBlurEffect()
        if #available(iOS 13.0, *) {
            blurEffect = UIBlurEffect(style: .systemThickMaterialDark)
        } else {
            blurEffect = UIBlurEffect(style: .dark)
        }
        
//        blurEffect = UIBlurEffect(style: .dark)
        blurEffectView = UIVisualEffectView(effect: blurEffect)
        blurEffectView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        blurEffectView.isUserInteractionEnabled = true
        
        super.init(presentedViewController: presentedViewController, presenting: presentingViewController)
        
        tapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(dismissController))
        blurEffectView.addGestureRecognizer(tapGestureRecognizer)
        
//        setupDimmingView()
    }
    
    override var frameOfPresentedViewInContainerView: CGRect {
        var frame: CGRect = .zero
        frame.size = size(forChildContentContainer: presentedViewController, withParentContainerSize: containerView!.bounds.size)
        
//        print("frameOfPresentedViewInContainerView -- containerView!.frame.height: ", containerView!.frame.height)
//        print("frameOfPresentedViewInContainerView -- presentedView!.frame.height: ", presentedView!.frame.height)
        
        switch direction {
        case .right:
            frame.origin.x = 30
        case .bottom:
            frame.origin.y = statusBarHeight + navigationBarHeight
//            frame.origin.y = containerView!.frame.height - presentedView!.frame.height
        }
//        let origin = CGPoint(x: 0, y: statusBarHeight)
//        let size = CGSize(width: containerView!.frame.width, height: containerView!.frame.height - statusBarHeight)
        
//        let origin = CGPoint(x: 30, y: 0)
//        let size = CGSize(width: containerView!.frame.width - 30, height: containerView!.frame.height)
//        return CGRect(origin: origin, size: size)

        /*
        guard var frame = containerView?.frame else { return .zero }
        
        switch detent {
        case .medium:
            frame.origin.y = frame.height / 2
            frame.size.height = frame.height / 2
        case .large:
            frame.origin.y = statusBarHeight + navigationBarHeight
            frame.size.height = frame.height
        }
        
        return frame
        */
        
        return frame
    }
    
    override func presentationTransitionWillBegin() {
        blurEffectView.alpha = 0
        containerView?.addSubview(blurEffectView)
        presentedViewController.transitionCoordinator?.animate(alongsideTransition: { _ in
            self.blurEffectView.alpha = 0.5
        }, completion: nil)
    }
    
    override func dismissalTransitionWillBegin() {
        presentedViewController.transitionCoordinator?.animate(alongsideTransition: { _ in
            self.blurEffectView.alpha = 0
        }, completion: { _ in
            self.blurEffectView.removeFromSuperview()
        })
    }
    
    override func containerViewWillLayoutSubviews() {
        super.containerViewWillLayoutSubviews()
        if direction == .bottom {
            presentedView!.roundCorners([.topLeft, .topRight], radius: 25)
        }
    }
    
    override func containerViewDidLayoutSubviews() {
        super.containerViewDidLayoutSubviews()
        presentedView?.frame = frameOfPresentedViewInContainerView
        blurEffectView.frame = containerView!.bounds
        
        if direction == .bottom {
            if !hasSetPointOrigin {
                hasSetPointOrigin = true
                pointOrigin = presentedView?.frame.origin
            }
            
            let swipeGestureRecognizer = UIPanGestureRecognizer(target: self, action: #selector(panGestureAction(_:)))
            swipeGestureRecognizer.delegate = self
            presentedView?.addGestureRecognizer(swipeGestureRecognizer)
        }
    }
    
    override func size(forChildContentContainer container: UIContentContainer, withParentContainerSize parentSize: CGSize) -> CGSize {
        switch direction {
        case .right:
            return CGSize(width: containerView!.frame.width - 30, height: containerView!.frame.height)
        case .bottom:
//            return CGSize(width: containerView!.frame.width, height: containerView!.frame.height - statusBarHeight)
            return CGSize(width: containerView!.frame.width, height: containerView!.frame.height - statusBarHeight - navigationBarHeight)

//            return CGSize(width: containerView!.frame.width, height: presentedView!.frame.height)
        }
    }
    
    
    // Pan gesture 사용해서 dismiss
    var pointOrigin: CGPoint?
    var hasSetPointOrigin = false
    
    @objc func panGestureAction(_ sender: UIPanGestureRecognizer) {
        guard let view = presentedView else { return }
        
        let translation = sender.translation(in: view)
        
        // 위로 슬라이드 방지
        guard translation.y >= 0 else { return }

        view.frame.origin = CGPoint(x: 0, y: self.pointOrigin!.y + translation.y)

        if sender.state == .ended {
            let dragVelocity = sender.velocity(in: view)
            if dragVelocity.y >= 800 {
                // Velocity fast enough to dismiss the uiview
                presentingViewController.dismiss(animated: true, completion: nil)
            } else {
                // Set back to original position of the view controller
                UIView.animate(withDuration: 0.3) {
                    view.frame.origin = self.pointOrigin ?? CGPoint(x: 0, y: 400)
                }
            }
        }
    }
    
    override func preferredContentSizeDidChange(forChildContentContainer container: UIContentContainer) {
        print("vieeeew container: ", container)
    }
    
    @objc func dismissController() {
        print("dismisscontroller")
        presentingViewController.dismiss(animated: true, completion: nil)
    }
}

extension FilterPresentationController: UIGestureRecognizerDelegate {
    func gestureRecognizer(_ gestureRecognizer: UIGestureRecognizer, shouldRecognizeSimultaneouslyWith otherGestureRecognizer: UIGestureRecognizer) -> Bool {
        if let scrollView = otherGestureRecognizer.view as? UIScrollView {
            return scrollView.contentOffset.y <= 0
        }
        return false
    }
}

extension UIView {
    /// 코너 라운딩
    func roundCorners(_ corners: UIRectCorner, radius: CGFloat) {
        let path = UIBezierPath(roundedRect: bounds, byRoundingCorners: corners, cornerRadii: CGSize(width: radius, height: radius))
        let mask = CAShapeLayer()
        mask.path = path.cgPath
        layer.mask = mask
    }
}

enum PresentationDirection {
    case right
    case bottom
}

```

```swift
@IBAction func showPopup() {
    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    let viewController = storyboard.instantiateViewController(withIdentifier: "FilterViewController")
    viewController.modalPresentationStyle = .custom
    viewController.transitioningDelegate = self
    present(viewController, animated: true, completion: nil)
}

extension MainViewController: UIViewControllerTransitioningDelegate {
    func presentationController(forPresented presented: UIViewController, presenting: UIViewController?, source: UIViewController) -> UIPresentationController? {
        FilterPresentationController(presentedViewController: presented, presenting: presenting)
    }
}
```