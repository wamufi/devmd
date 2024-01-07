---
boostnote:
  createdAt: '2020-07-07T01:58:59.009Z'
  updatedAt: '2020-07-07T02:41:07.139Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Codable with Nested JSON
  tags:
    - Networking
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 42134d03-dda3-4a75-b7fb-311378a982d8
  storage: d112f8ec1e85e056a09d
---

Codable with Nested JSON
---
[Using Codable With Nested JSON Is Both Easy And Fun!](https://medium.com/@nictheawesome/using-codable-with-nested-json-is-both-easy-and-fun-19375246c9ff)

JSON
```json
{
  "mcustNo" : "0007817698",
  "mktAgrList" : {
    "agreeYn" : "N",
    "mktAgrTpGbCd" : "08"
  }
}
```

Codable
```swift
struct RoulettePushParam: Codable {
    let custNo: String
    let typeCode: String
    let agreeYn: String
    
    enum CodingKeys: String, CodingKey {
        case custNo = "mcustNo"
        case agreeList = "mktAgrList"
    }
    
    enum AgreeKeys: String, CodingKey {
        case typeCode = "mktAgrTpGbCd"
        case agreeYn
    }
    
    // Encodable
    func encode(to encoder: Encoder) throws {
        var baseContainer = encoder.container(keyedBy: CodingKeys.self)
        try baseContainer.encode(custNo, forKey: .custNo)
        
        var agreeListContainer = baseContainer.nestedContainer(keyedBy: AgreeKeys.self, forKey: .agreeList)
        try agreeListContainer.encode(typeCode, forKey: .typeCode)
        try agreeListContainer.encode(agreeYn, forKey: .agreeYn)
    }
    
    // Decodable
    required init(from decoder: Decoder) throws {
        let baseContainer = decoder.container(keyedBy: CodingKeys.self)
        let custNo = try baseContainer.decode(String.self, forKey: .custNo)
        
        let agreeListContainer = baseContainer.nestedContainer(keyedBy: AgreeKeys.self, forKey. agreeList)
        let typeCode = try agreeListContainer.decode(String.self, forKey: .typeCode)
        let agreeYn = try agreeListContainer.decode(String.self, forKey: .agreeYn)
    }

    /*
    // Only One CodingKey
    enum CodingKeys: String, CodingKey {
        case custNo = "mcustNo"
        case agreeList = "mktAgrList"
        case typeCode = "mktAgrTpGbCd"
        case agreeYn
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(custNo, forKey: .custNo)
        
        var agreeListContainer = container.nestedContainer(keyedBy: CodingKeys.self, forKey: .agreeList)
        try agreeListContainer.encode(typeCode, forKey: .typeCode)
        try agreeListContainer.encode(agreeYn, forKey: .agreeYn)
    }
 */
}
```

Using
```swift
let Info = RoulettePushParam(custNo: "0007817698", typeCode: "08", agreeYn: "Y")
```

Print
```swift
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
let encoded = try! encoder.encode(info)
print(String(data: encoded, encoding: .utf8)!)
```