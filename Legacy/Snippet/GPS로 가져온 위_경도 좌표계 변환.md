---
boostnote:
  createdAt: '2022-03-28T12:19:32.151Z'
  updatedAt: '2022-03-28T12:21:19.035Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: GPS로 가져온 위/경도 좌표계 변환
  tags:
    - Android
    - Kotlin
    - Location
  linesHighlighted: []
  isStarred: false
  key: 095cce37-99c3-4e86-9d1b-7f76f0d5471a
  storage: d112f8ec1e85e056a09d
---

GPS로 가져온 위/경도 좌표계 변환
---
공공데이터에서는 TM 좌표를 좀 쓰는 것 같다. 적어도 미세먼지 api는 사용한다.

Proj4J를 사용해서 변환했다.
```kotlin
fun transformCoordinate(latitude: Double, longitude: Double) {
    val factory = CRSFactory()
    val WGS84 = factory.createFromName("epsg:4326")
    val GRS80 = factory.createFromName("epsg:5181")

    val transform = CoordinateTransformFactory().createTransform(WGS84, GRS80)
    _coordinate.value = transform.transform(ProjCoordinate(longitude, latitude), ProjCoordinate())
}
```

기타 좌표계는 여기 참고
https://www.osgeo.kr/17
