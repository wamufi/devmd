---
boostnote:
  createdAt: '2022-05-04T02:45:41.959Z'
  updatedAt: '2022-05-18T08:02:21.799Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Alamofire + Combine + MVVM
  tags:
    - iOS
    - Swift
    - Combine
    - Networking
    - Alamofire
    - MVVM
  linesHighlighted: []
  isStarred: false
  key: ec27301c-927b-4bf8-a393-f735816913e9
  storage: d112f8ec1e85e056a09d
---

Alamofire + Combine + MVVM
---
Alamofire에서 Combine 사용 시 에러를 alamofire 쪽에서 처리했다.

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
        
        struct SameName: Decodable {
            let keyword: String
            let region: [String]
            let selectedRegion: String

            enum CodingKeys: String, CodingKey {
                case keyword, region
                case selectedRegion = "selected_region"
            }
        }
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
import Combine

struct NetworkError: Error {
  let initialError: AFError
  let backendError: BackendError?
}

struct BackendError: Codable, Error {
    var status: String
    var message: String
}

class KakaoAPIManager {
    static let shared = KakaoAPIManager()
    
    /// 키워드로 장소 검색하기
    /// - note: Combine 사용
    /// - seealso:
    /// [kakao developers 개발 가이드](https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-keyword)
    func searchKeyword(query: String) -> AnyPublisher<DataResponse<LocalSearch, NetworkError>, Never> {
        let parameters = LocalSearchParameters(query: query, page: 1, size: 15)

        return AF.request(KakaoRouter.keyword(parameters)).validate()
            .publishDecodable(type: LocalSearch.self).map { response in
                response.mapError { error in
                    let backendError = response.data.flatMap { try? JSONDecoder().decode(BackendError.self, from: $0)}
                    return NetworkError(initialError: error, backendError: backendError)
                }
            }
            .receive(on: DispatchQueue.main).eraseToAnyPublisher()
    }
}
```

SearchKeywordViewModel.swift
```swift
import UIKit
import Combine

class SearchKeywordViewModel: ObservableObject {
    @Published var keyword: String = ""
    @Published var dataSource: [PlaceViewModel] = []
    
    /// 선택된 검색 결과들
    @Published var selectedDocuments = [PlaceViewModel]()
    /// 추가된 검색 결과들
    @Published var addedDocuments = [LocalSearch.Document]()
    
    var apiManager: KakaoAPIManager
    private var disposables = Set<AnyCancellable>()
    
    init(apiManager: KakaoAPIManager) {
        self.apiManager = apiManager
    }
    
    func search(forKeyword keyword: String) {
        apiManager.searchKeyword(query: keyword)
            .sink { response in
                if response.error != nil {
                    // 에러 핸들링
                    debugPrint("error: \(response.error)")
                    self.dataSource = []
                } else {
                    if let documents = response.value?.documents {
                        // .map으로 변환
                        self.dataSource = documents.map(PlaceViewModel.init)
                    }
                }
            }.store(in: &disposables)

    }
}

```

PlaceViewModel.swift
```swift
import Foundation
import CoreLocation

struct PlaceViewModel {
    private let item: LocalSearch.Document
    
    init(item: LocalSearch.Document) {
        self.item = item
    }
    
    var id: String {
        return item.id
    }
    
    var placeName: String {
        return item.placeName
    }
    
    var categoryName: String {
        var returnString = item.categoryGroupName
        if returnString.isEmpty {
            let category = item.categoryName
            
            let regex = "(?:[^ \\>](?!\\>))+$"
            let expression = try? NSRegularExpression(pattern: regex, options: .caseInsensitive)
            let categoryRange = NSRange(location: 0, length: category.utf16.count)
            if let match = expression?.firstMatch(in: category, options: [], range: categoryRange) {
                returnString = category.substring(with: match.range) ?? ""
            }
        }
        
        return returnString
    }
    
    var phone: String {
        return item.phone
    }
    
    var addressName: String {
        if item.addressName.isEmpty {
            return item.roadAddressName
        } else {
            return item.addressName
        }
    }
    
    var xDouble: Double {
        return Double(item.x) ?? 0
    }
    
    var yDouble: Double {
        return Double(item.y) ?? 0
    }
    
    /// 좌표값
    var coordinate: CLLocationCoordinate2D {
        return CLLocationCoordinate2D(latitude: yDouble, longitude: xDouble)
    }
    
    var placeUrl: URL? {
        return URL(string: item.placeURL)
    }
    
    var distanceDouble: Double {
        return Double(item.distance) ?? 0
    }
}
```

ViewController.swift
```swift
import UIKit
import Combine

class ViewController: UIViewController {
    
    private var searchController: UISearchController!
    private var resultsViewController: SearchViewController!

    private lazy var viewModel = SearchKeywordViewModel(apiManager: KakaoAPIManager.shared)
    var disposables = Set<AnyCancellable>()

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        setupSearchController()
        setupBindings()
    }

    private func setupSearchController() {
        resultsViewController = storyboard?.instantiateViewController(withIdentifier: "SearchResultsViewController") as? SearchResultsViewController
        resultsViewController.viewModel = viewModel
        
        searchController = UISearchController(searchResultsController: resultsViewController)
        searchController.delegate = self
        searchController.searchBar.delegate = self
        definesPresentationContext = true
        
        navigationItem.searchController = searchController
    }
    
    private func setupBindings() {
        viewModel.$dataSource.sink { values in
            if values.count > 0 {
                self.updateSearchResults(response: values)
            }
        }.store(in: &disposables)
        
        viewModel.$selectedDocuments.sink { values in
            if values.count > 0 {
                self.selectedDocuments = values
                self.navigationController?.navigationBar.topItem?.prompt = "\(values.count)개의 장소가 새로운 코스에 추가됨"
            }
        }.store(in: &disposables)
    }
    
    private func searchLocal(_ text: String) {
        if text.count <= 0 {
            print("search text null")
            return
        }

        viewModel.keyword = text
        viewModel.search(forKeyword: text)
    
    private func updateSearchResults(response: [PlaceViewModel]) {
        if let resultsController = searchController.searchResultsController as? SearchResultsViewController {
            resultsController.searchedPlaces = response
            resultsController.tableView.reloadData()
            resultsController.addMarkers()
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