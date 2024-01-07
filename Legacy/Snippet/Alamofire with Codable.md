---
boostnote:
  createdAt: '2020-07-23T07:05:10.903Z'
  updatedAt: '2020-07-28T09:19:49.286Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Alamofire with Codable
  tags:
    - Alamofire
    - Networking
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 2e293368-7053-4f50-8518-8eb9193e954e
  storage: d112f8ec1e85e056a09d
---

Alamofire with Codable
---
```swift
/// encodable 이용한 데이터 전송
///
/// - parameters:
///   - url: url 주소
///   - info: Encodable body
func sendDataWithEncodable<Parameters: Encodable>(url: String, info: Parameters, completion: @escaping ([String: Any]?) -> ()) {
    print("info: ", info)
    
    let request = AF.request(url, method: .post, parameters: info, encoder: JSONParameterEncoder.default, headers: headers)
    request.validate().responseJSON { response in
        debugPrint("response: ", response)
        guard let json = response.value as? [String: Any] else { return }
        print("json: ", json)
        completion(json)
    }
}

```

```swift
extension Encodable {
    func toJSONData() -> Data? { try? JSONEncoder().encode(self) }
}
```