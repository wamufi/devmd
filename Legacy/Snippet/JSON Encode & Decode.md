---
boostnote:
  createdAt: '2022-05-31T09:26:17.087Z'
  updatedAt: '2022-05-31T09:34:56.907Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: JSON Encode & Decode
  tags:
    - iOS
    - Swift
    - JSON
  linesHighlighted: []
  isStarred: false
  key: 88d0aa65-ed8b-469e-af9a-769924712c64
  storage: d112f8ec1e85e056a09d
---

JSON Encode & Decode
---
Struct
```swift
struct PlaceViewModel {
    let item: LocalSearch.Document
    
    init(item: LocalSearch.Document) {
        self.item = item
    }
}

struct PlaceViewModelEncodable: Encodable {
    let items: [LocalSearch.Document]
    
    init(items: [LocalSearch.Document]) {
        self.items = items
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.unkeyedContainer()
        for item in items {
            try container.encode(item) // 배열로 만들기 위해 필요
        }
    }
}
```

Encode
```swift
let encodable = PlaceViewModelEncodable(items: addedDocuments.map { $0.item }) // [PlaceViewModel] -> [LocalSearch.Document] 변환
do {
    let jsonData = try JSONEncoder().encode(encodable)
    let jsonString = String(data: jsonData, encoding: .utf8)!
    tripRoad = jsonString
} catch {
    print(String(describing: error))
}
```

JSON
```json
[{"id":"977211586","place_url":"http:\/\/place.map.kakao.com\/977211586","phone":"02-2251-8010","address_name":"서울 종로구 청진동 246","road_address_name":"서울 종로구 종로3길 17","x":"126.97892048108426","y":"37.5709805434398","category_group_name":"카페","category_name":"음식점 > 카페","place_name":"펠트커피 광화문점","category_group_code":"CE7","distance":""},
{"id":"27187492","place_url":"http:\/\/place.map.kakao.com\/27187492","phone":"02-2251-8125","address_name":"서울 종로구 청진동 246","road_address_name":"서울 종로구 종로3길 17","x":"126.978927265534","y":"37.5710066735133","category_group_name":"카페","category_name":"음식점 > 카페","place_name":"포비 광화문점","category_group_code":"CE7","distance":""}]
```

Decode
```swift
let decoder = JSONDecoder()
do {
    let decode: [LocalSearch.Document] = try decoder.decode([LocalSearch.Document].self, from: tripRoad.data(using: .utf8)!)
    addedDocuments = decode.map(PlaceViewModel.init) // [LocalSearch.Document] -> [PlaceViewModel] 변환

    updateUI()
} catch {
    print(String(describing: error))
}
```