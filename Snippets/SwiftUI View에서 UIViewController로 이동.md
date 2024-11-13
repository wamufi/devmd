---
title: SwiftUI View에서 UIViewController로 이동
date: 2024-11-13
time: 15:35:13
tags:
- swift
- ios
- swiftui
- uikit
---
네비게이션 바가 두개로 나오거나 백 버튼이 안 나오거나 기타 등등

FirstScreen.swift
```swift
import SwiftUI

struct FirstScreen: View {
    var body: some View {
        NavigationView {
            VStack {
                Text("First Screen")
                    .font(.largeTitle)
                    .padding()

                // NavigationLink로 SecondVC로 이동
                NavigationLink("Go to Second Screen", destination: SecondVCWrapper())
                    .padding()
                    .background(Color.blue)
                    .foregroundColor(.white)
                    .cornerRadius(8)
            }
        }
    }
}
```

SecondViewController.swift
```swift
import UIKit

class SecondViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()

		// 스토리보드에서 설정해도 된다
        self.title = "Second Screen"
        self.view.backgroundColor = .white

        let rightButton = UIBarButtonItem(title: "Right", style: .plain, target: self, action: #selector(rightButtonTapped))
        self.navigationItem.rightBarButtonItem = rightButton
        // 스토리보드에서 설정해도 된다
    }

    @objc func rightButtonTapped() {
        print("Right button tapped")
    }
}
```

SecondViewControllerRepresentable.swift
```swift
import SwiftUI

struct SecondViewControllerRepresentable: UIViewControllerRepresentable {
    class Coordinator {
        var parentObserver: NSKeyValueObservation?
    }
    
    func makeUIViewController(context: Context) -> SecondViewController {
        let storyboard = UIStoryboard(name: "Main", bundle: nil)
        let viewController = storyboard.instantiateViewController(withIdentifier: "SecondViewController") as! SecondViewController
        context.coordinator.parentObserver = viewController.observe(\.parent, changeHandler: { vc, _ in
            vc.parent?.title = vc.title
            vc.parent?.navigationItem.rightBarButtonItems = vc.navigationItem.rightBarButtonItems
        })
       return viewController
    }

    func updateUIViewController(_ uiViewController: UINavigationController, context: Context) {
        // 필요시 업데이트 처리
    }
    
    func makeCoordinator() -> Self.Coordinator { Coordinator() }
}

```

https://stackoverflow.com/a/68567095