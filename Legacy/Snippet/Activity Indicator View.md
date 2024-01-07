---
title: "Activity Indicator View"
date: 2023-07-31
time: 16:54:10
tags:
  - ios
  - swift
  - uiactivityindicatorview
id: 20230731165410
---

UIActivityIndicatorView
---
```swift
struct IndicatorView {
    let view: UIView!
    let viewForActivityIndicatorView = UIView()
    let backgroundView = UIView()
    let navigationController: UINavigationController?
    let tabBarController: UITabBarController?
    let activityIndicatorView = UIActivityIndicatorView()
    let textLabel = UILabel()
    
    var navigationBarHeight: CGFloat {
        return navigationController?.navigationBar.frame.size.height ?? 0
    }
    var tabBarHeight: CGFloat {
        return tabBarController?.tabBar.frame.size.height ?? 0
    }
    
    func showIndicatorView(loadingText: String) {
        viewForActivityIndicatorView.frame = CGRect(x: 0, y: 0, width: 100, height: 100)
        viewForActivityIndicatorView.center = CGPoint(x: view.frame.size.width / 2, y: (view.frame.size.height - navigationBarHeight - tabBarHeight) / 2)
        viewForActivityIndicatorView.cornerRadius = 10
        viewForActivityIndicatorView.backgroundColor = .clear
        backgroundView.addSubview(viewForActivityIndicatorView)
        
        textLabel.textColor = .gray
        textLabel.text = loadingText
        textLabel.font = .systemFont(ofSize: 14, weight: .light)
        textLabel.sizeToFit()
        textLabel.center = CGPoint(x: viewForActivityIndicatorView.frame.size.width / 2, y: viewForActivityIndicatorView.frame.size.height / 2 + 30)
        viewForActivityIndicatorView.addSubview(textLabel)
        
        activityIndicatorView.center = CGPoint(x: viewForActivityIndicatorView.frame.size.width / 2, y: (viewForActivityIndicatorView.frame.size.height) / 2 - 10)
        activityIndicatorView.hidesWhenStopped = true
        activityIndicatorView.style = .large
        viewForActivityIndicatorView.addSubview(activityIndicatorView)
        
        backgroundView.frame = CGRect(x: 0, y: 0, width: view.window?.windowScene?.screen.bounds.width ?? 0, height: view.window?.windowScene?.screen.bounds.height ?? 0)
        backgroundView.backgroundColor = .clear
        
        view.addSubview(backgroundView)
        
        activityIndicatorView.startAnimating()
    }
    
    func hideIndicatorView() {
        activityIndicatorView.stopAnimating()
        activityIndicatorView.removeFromSuperview()
        textLabel.removeFromSuperview()
        viewForActivityIndicatorView.removeFromSuperview()
        backgroundView.removeFromSuperview()
    }
}
```
사용법
```swift
private var indicatorView: IndicatorView!

indicatorView = IndicatorView(view: view, navigationController: navigationController, tabBarController: tabBarController)
indicatorView.showIndicatorView(loadingText: "로드 중")

self.indicatorView.hideIndicatorView()
```
