---
boostnote:
  createdAt: '2022-05-04T02:57:15.842Z'
  updatedAt: '2022-05-04T03:26:38.019Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Alamofire + Concurrency
  tags:
    - iOS
    - Swift
    - Networking
    - async/await
    - Alamofire
  linesHighlighted: []
  isStarred: false
  key: 6b4670f5-eb4c-4f36-a9d5-42a42be9637c
  storage: d112f8ec1e85e056a09d
---

Alamofire + Concurrency
---
LocalSearchStruct.swift
```swift
import Foundation

// MARK: - Encodable
struct LocalSearchParameters: Encodable {
    let query: String
    let page: Int?
    let size: Int?
}

// MARK: - Decodable
struct LocalSearch: Decodable {
    let documents: [Document]
    let meta: Meta
}

struct Document: Decodable, Identifiable {
//    var identity = UUID()
    let addressName, categoryGroupCode, categoryGroupName, categoryName: String
    let distance, id, phone, placeName: String
    let placeURL: String
    let roadAddressName, x, y: String

    enum CodingKeys: String, CodingKey {
        case addressName = "address_name"
        case categoryGroupCode = "category_group_code"
        case categoryGroupName = "category_group_name"
        case categoryName = "category_name"
        case distance, id, phone
        case placeName = "place_name"
        case placeURL = "place_url"
        case roadAddressName = "road_address_name"
        case x, y
    }
}

struct Meta: Decodable {
    let isEnd: Bool
    let pageableCount: Int
    let sameName: SameName
    let totalCount: Int
    
    enum CodingKeys: String, CodingKey {
        case isEnd = "is_end"
        case pageableCount = "pageable_count"
        case sameName = "same_name"
        case totalCount = "total_count"
    }
}

struct SameName: Decodable {
    let keyword: String
    let region: [String]
    let selectedRegion: String

    enum CodingKeys: String, CodingKey {
        case keyword, region
        case selectedRegion = "selected_region"
    }
}
```

KakaoRouter.swift
```swift
import Foundation
import Alamofire

enum KakaoRouter {
    case keyword(LocalSearchParameters)
    
    var baseURL: String {
        "https://dapi.kakao.com/v2"
    }
    
    var path: String {
        switch self {
        case .keyword:
            return "local/search/keyword.json"
        }
    }
    
    var headers: HTTPHeaders {
       return ["Authorization": "KakaoAK de419495028a80e8cbbd3e23e8c3e830"]
    }
}

extension KakaoRouter: URLRequestConvertible {
    func asURLRequest() throws -> URLRequest {
        let url = try baseURL.asURL().appendingPathComponent(path)
        
        var request = URLRequest(url: url)
        request.method = .get
        request.headers = headers
        
        switch self {
        case let .keyword(parameters):
            request = try URLEncodedFormParameterEncoder().encode(parameters, into: request)
        }
        
        return request
    }
}
```

KakaoApiManager.swift
```swift
import Foundation
import Alamofire

class KakaoAPIManager {
    static let shared = KakaoAPIManager()
    
    private let session: Session = {
        let configuration = URLSessionConfiguration.default
        configuration.timeoutIntervalForRequest = 10
        configuration.timeoutIntervalForResource = 10
        return Session(configuration: configuration)
    }()
    
    /// 키워드로 장소 검색하기
    /// - note: Concurrency 사용
    /// - seealso:
    /// [kakao developers 개발 가이드](https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-keyword)
    func searchKeywordConcurrency(query: String) async throws -> LocalSearch {
        let parameters = LocalSearchParameters(query: query, page: 1, size: 15)

        return try await session.request(KakaoRouter.keyword(parameters)).serializingDecodable(LocalSearch.self).value
    }
}
```

KeywordViewModel.swift
```swift
import Foundation
import Combine
import CoreLocation

// 기존 Combine 프로젝트 재활용이라서 @Published, ObservableObject 있는데 사실 필요 없을 듯?
class KeywordViewModel: ObservableObject {
    @Published var keyword: String = ""
    @Published var document = [Document]()
    
    @Published private(set) var placeName = ""
    @Published private(set) var categoryName = ""
    @Published private(set) var address = ""
    @Published private(set) var url = ""
    @Published private(set) var location: CLLocation? = nil
    
    var apiManager: KakaoAPIManager
    
    init(apiManager: KakaoAPIManager) {
        self.apiManager = apiManager
    }
    
    func search(query: String) {
        self.keyword = query
        
        Task {
            try? await searchKeywordConcurrency()
        }
    }
    
    private func searchKeywordConcurrency() async throws {
        let places = try? await apiManager.searchKeywordConcurrency(query: keyword)
        guard let value = places?.documents else { return }
        self.document = value
    }
}
```

ViewController.swift
```swift
import UIKit
//import Alamofire
import Combine

// 기존 Combine 프로젝트 재활용
class ViewController: UIViewController {
    
    private var searchController: UISearchController!
    private var resultsViewController: SearchViewController!

    private var viewModel = KeywordViewModel(apiManager: KakaoAPIManager.shared)
    var cancelBag = Set<AnyCancellable>()

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
//        setupBindings()
        
        resultsViewController = storyboard?.instantiateViewController(withIdentifier: "SearchViewController") as? SearchViewController
        
        searchController = UISearchController(searchResultsController: resultsViewController)
        searchController.delegate = self
        searchController.searchBar.delegate = self
//        searchController.searchResultsUpdater = self
        
        navigationItem.searchController = searchController
    }

    private func searchLocal(_ text: String) {
        if text.count <= 0 {
            print("search text null")
            return
        }
        
//        KakaoAPIManager.shared.searchKeyword(query: text) { response in
//            print("ViewController -- searchLocal")
//            debugPrint(response)
//        }
        
        viewModel.keyword = text
//        viewModel.searchKeyword(query: text)
        viewModel.search(query: text)
        viewModel.$document.receive(on: DispatchQueue.main)
            .sink(receiveValue: { [weak self] _ in
                self?.updateSearchResults(response: (self?.viewModel.document)!)
            })
            .store(in: &self.cancelBag)
    }
    
    private func updateSearchResults(response: [Document]) {
        if let resultsController = searchController.searchResultsController as? SearchViewController {
            resultsController.searchedPlaces = response
            resultsController.tableView.reloadData()
        }
    }
}


// MARK: - UISearchControllerDelegate
extension ViewController: UISearchControllerDelegate {
    
}

// MARK: - UISearchResultsUpdating
extension ViewController: UISearchResultsUpdating {
    // 실시간 검색
    func updateSearchResults(for searchController: UISearchController) {
        print("updateSearchResults: \(searchController)")
    }
}

// MARK: - UISearchBarDelegate
extension ViewController: UISearchBarDelegate {
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        searchLocal(searchBar.text ?? "")
        searchBar.resignFirstResponder()
    }
}

```