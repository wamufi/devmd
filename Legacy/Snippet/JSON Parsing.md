---
boostnote:
  createdAt: '2020-06-05T07:22:15.615Z'
  updatedAt: '2020-06-10T02:28:09.187Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: JSON Parsing
  tags:
    - Alamofire
    - Networking
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 0afa1d45-475c-41aa-abe1-f13fbd97201a
  storage: d112f8ec1e85e056a09d
---

JSON Parsing
---
```swift
class FetchClient: NSObject {
    // URLSession
    // https://developer.apple.com/documentation/foundation/url_loading_system
    func fetchFilmsWithURLSession(completion: @escaping ([NSDictionary]?) -> Void) {
        let url = URL(string: "https://itunes.apple.com/us/rss/topmovies/limit=25/json")!

        let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
            if let error = error {
                print("error: " + error.localizedDescription)
            } else if let data = data {
                do {
                    if let json = try JSONSerialization.jsonObject(with: data, options:.allowFragments) as? NSDictionary {
                        print(json)
                        if let films = json.value(forKeyPath: "feed.entry") as? [NSDictionary] {
                            completion(films)
                            return
                        }
                    }
                } catch {

                }
            }
        }
        task.resume()
    }

    // Alamofire
    // https://github.com/Alamofire/Alamofire/blob/master/Documentation/Usage.md
    func fetchFilmsWithAlamofire(completion: @escaping([NSDictionary]?) -> Void) {
        let urlString = "https://itunes.apple.com/us/rss/topmovies/limit=25/json"

        AF.request(urlString).validate().responseJSON { (response) in
            guard let json = response.value as? NSDictionary else { return }
                print(json)
            if let films = json.value(forKeyPath: "feed.entry") as? [NSDictionary] {
                completion(films)
            }
        }
    }

    // Alamofire with Codable
    func fetchEvents(completion: @escaping([Event]?) -> Void) {
        let urlString = "https://m.h-point.co.kr:29845/cp/camp/eventCampDtlListV2.nhd"
//        let info = ["mcustNo": "0007817698", "ptcoId": ""]
        let info = Info(mcustNo: "0007817698", ptcoId: "")
        let headers: HTTPHeaders = [
            "Content-Type": "application/json",
            "connId": "",
            "authKey": "",
            "teOpsyGbcd": "02",
            "appVer": "1.8.8",
            "awakenYn": "N",
            "strAutoLogin": "N",
            "mblCrdNo": ""
        ]
        
        let request = AF.request(urlString, method: .post, parameters: info, encoder: JSONParameterEncoder.default, headers: headers)

        request.validate().responseDecodable(of: Events.self) { (response) in
            guard let events = response.value else { return }
            completion(events.all)
        }
    }
}

struct Info: Encodable {
    let mcustNo: String
    let ptcoId: String
}

struct Events: Decodable {
    var all: [Event]
    
    enum CodingKeys: String, CodingKey {
        case all = "camp18List"
    }
}

struct Event: Decodable {
    let name: String
    let startDate: String
    let endDate: String

    enum CodingKeys: String, CodingKey {
        case name = "eventNm"
        case startDate = "eventStDt"
        case endDate = "eventEdDt"
    }
}
        
```