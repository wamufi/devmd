---
boostnote:
  createdAt: '2022-10-12T07:39:21.081Z'
  updatedAt: '2022-10-12T08:00:13.521Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIViewCotnroller를 xib 없이 생성
  tags:
    - Swift
    - iOS
    - UIViewController
  linesHighlighted: []
  isStarred: false
  key: 1a88d132-23d9-4ba4-9d6c-138a0cf0539d
  storage: d112f8ec1e85e056a09d
---

UIViewCotnroller를 xib 없이 생성
---
```swift
class RollingBannerViewController: BaseViewController {
    
    private let tableView = UITableView()
    private var navigationView: NavigationView!
    
    var viewModel: TopBannerViewModel!
    
    init(viewModel: TopBannerViewModel) {
        self.viewModel = viewModel
        super.init(nibName: nil, bundle: nil)
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()

        initUI()
    }
    
    private func initUI() {
        if #available(iOS 13.0, *) {
            view.backgroundColor = .systemBackground
        } else {
            view.backgroundColor = .white
        }
        
        tableView.delegate = self
        tableView.dataSource = self
        tableView.separatorStyle = .none
        tableView.register(EventListTableViewCell.self, forCellReuseIdentifier: "EventListTableViewCell")
        
        view.addSubview(tableView)
        tableView.snp.makeConstraints { make in
            make.leading.trailing.equalTo(view.safeAreaLayoutGuide)
            make.bottom.equalToSuperview()
        }
        
        initNavigationView()
    }
    
    /// 네비게이션 뷰 초기화
    private func initNavigationView() {
        navigationView = Bundle.main.loadNibNamed("NavigationView", owner: self, options: nil)?.first as? NavigationView
        navigationView.isOnlyBackButton = true
        navigationView.backLabel.text = "전체보기"
        view.addSubview(navigationView)
        
        navigationView.snp.makeConstraints { make in
            make.leading.trailing.top.equalTo(view.safeAreaLayoutGuide)
            make.bottom.equalTo(tableView.snp.top)
        }
    }

}

extension RollingBannerViewController: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return viewModel.getTopBannerCount()
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "EventListTableViewCell", for: indexPath) as! EventListTableViewCell
        cell.eventImageView.setImageUrl(viewModel.getMoreImgUrl(indexPath.row))
        
        return cell
    }
    
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        linkPageList("", viewModel.getMoreLinkUrl(indexPath.row))
    }
}
```

```swift
@IBAction func rollingBannerAction(_ sender: UIButton) {
    let viewController = RollingBannerViewController(viewModel: topBannerViewModel)
    parentViewController?.navigationController?.pushViewController(viewController, animated: true)
}
```
