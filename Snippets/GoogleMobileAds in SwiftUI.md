---
title: GoogleMobileAds in SwiftUI
date: 2024-08-05
time: 03:16:44
tags:
	- swiftui
	- google
	- admob
---
구글에서 올라온 샘플 코드는 부르는 쪽에 `.task { await function() }` 코드가 있을 경우 로드를 못했다.

BannerViewController.swift의 코드를 ChatGPT한테 물어보니
1. **뷰의 크기 업데이트를 정확히 감지하지 못할 수 있음**
    `viewWillTransition(to:with:)` 메서드는 뷰 컨트롤러의 화면 크기가 변경될 때 호출됩니다. 그러나, `coordinator.animate`와 `completion` 클로저 안에서 크기 변경을 감지하기 어려울 수 있습니다.
    
2. **뷰의 프레임을 정확히 계산하지 못할 수 있음**
    `view.frame.inset(by: view.safeAreaInsets).size.width`는 `viewDidAppear(_:)` 및 `viewWillTransition(to:with:)`에서 올바르게 동작할 수 있지만, 화면 회전과 같은 복잡한 상황에서는 정확한 크기를 보장하기 어렵습니다.
    
3. **delegate 호출 시점 문제**
    화면 회전 시 delegate 호출이 필요하지만, 특정 상황에서 호출 시점이 잘못될 수 있습니다.
이런 문제점이 있다고 한다.

BannerViewController.swift
```swift
import UIKit

protocol BannerViewControllerWidthDelegate: AnyObject {
    func bannerViewController(_ bannerViewController: BannerViewController, didUpdate width: CGFloat)
}

class BannerViewController: UIViewController {
    weak var delegate: BannerViewControllerWidthDelegate?

    override func viewDidLoad() {
        super.viewDidLoad()
        updateWidth()
    }

    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        updateWidth()
    }

    private func updateWidth() {
        // Get the width of the view minus the safe area insets
        let width = view.frame.inset(by: view.safeAreaInsets).size.width
        delegate?.bannerViewController(self, didUpdate: width)
    }
}
```

BannerView.swift
```swift
import SwiftUI
import GoogleMobileAds

struct BannerView: UIViewControllerRepresentable {
    
    @Binding var viewWidth: CGFloat
    private let bannerView = GADBannerView()
    let adUnitID: String!
    
    func makeUIViewController(context: Context) -> some UIViewController {
        let bannerViewController = BannerViewController()
        bannerView.adUnitID = adUnitID
        bannerView.rootViewController = bannerViewController
        bannerViewController.delegate = context.coordinator
        bannerViewController.view.addSubview(bannerView)
        
        return bannerViewController
    }
    
    func updateUIViewController(_ uiViewController: UIViewControllerType, context: Context) {
        guard viewWidth != .zero else { return }
        
        // Request a banner ad with the updated viewWidth.
        bannerView.adSize = GADCurrentOrientationAnchoredAdaptiveBannerAdSizeWithWidth(viewWidth)
        
        let request = GADRequest()
        bannerView.load(request)
    }

    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    internal class Coordinator: NSObject, BannerViewControllerWidthDelegate  {
        
        let parent: BannerView
        
        init(_ parent: BannerView) {
            self.parent = parent
        }
        
        func bannerViewController(_ bannerViewController: BannerViewController, didUpdate width: CGFloat) {
            DispatchQueue.main.async {
                self.parent.viewWidth = width
                print("Updated width: \(width)")
            }
        }
    }
}
```

ContentView.swift
```swift
struct ContentView: View {
	@State private var bannerViewWidth: CGFloat = UIScreen.main.bounds.width

	var body: some View {
		VStack {
			Form {
				Section(Text("Section")) {
					List {
						// ForEach
					}
					.task {
						await viewModel.fetchData()
					}
				}
			}
			
			Spacer().frame(height: 0)
			
			BannerView(viewWidth: $bannerViewWidth, adUnitID: bannerPubId(CAP_PUB_ID_SHOP))
				.frame(height: GADCurrentOrientationAnchoredAdaptiveBannerAdSizeWithWidth(UIScreen.main.bounds.width).size.height)
		}
	}
}
```