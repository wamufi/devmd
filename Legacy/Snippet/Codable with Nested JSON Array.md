---
boostnote:
  createdAt: '2020-07-07T02:08:30.155Z'
  updatedAt: '2020-07-07T02:17:16.270Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Codable with Nested JSON Array
  tags:
    - Networking
    - Swift
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 82fc947a-6f75-40b5-99f0-1af8445f0f3d
  storage: d112f8ec1e85e056a09d
---

Codable with Nested JSON Array
---
[Using Codable With Nested Arrays Is Easier And More Fun Than You Think](https://medium.com/@nictheawesome/using-codable-with-nested-objects-is-easier-and-more-fun-than-you-think-8c9edfa29ed7)

JSON
```json
{
  "mcustNo" : "0007817698",
  "mktAgrList" : [
    {
      "agreeYn" : "N",
      "mktAgrTpGbCd" : "08"
    }
  ]
}
```

Codable
```swift
struct MarketingAgrees: Encodable {
    let typeCode: String
    let agreeYn: String
    
    enum CodingKeys: String, CodingKey {
        case typeCode = "mktAgrTpGbCd"
        case agreeYn
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(typeCode, forKey: .typeCode)
        try container.encode(agreeYn, forKey: .agreeYn)
    }
}

struct RoulettePushParam: Encodable {
    let custNo: String
    let agreeList: [MarketingAgrees]
    
    enum CodingKeys: String, CodingKey {
        case custNo = "mcustNo"
        case agreeList = "mktAgrList"
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(custNo, forKey: .custNo)
        try container.encode(agreeList, forKey: .agreeList)
    }
}
```

Using
```swift
let pushInfo = RoulettePushParam(custNo: "0007817698", agreeList: [MarketingAgrees(typeCode: "08", agreeYn: isAgreeString)])
```

Print
```swift
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
let encoded = try! encoder.encode(info)
print(String(data: encoded, encoding: .utf8)!)
```