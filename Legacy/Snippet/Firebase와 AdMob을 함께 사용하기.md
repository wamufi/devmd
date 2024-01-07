---
boostnote:
  createdAt: '2022-01-10T05:42:20.156Z'
  updatedAt: '2022-01-27T09:52:42.801Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Firebase와 AdMob을 함께 사용하기
  tags:
    - iOS
    - Swift
    - Firebase
    - AdMob
  linesHighlighted: []
  isStarred: false
  key: 179ab32c-c421-4a7f-bfc4-76ae58907cc1
  storage: d112f8ec1e85e056a09d
---

Firebase와 AdMob을 함께 사용하기
---
Firebase는 Swift Package Manager로 설치하고 AdMob은 수동 설치할 때, Firebase는 그냥 시키는 대로 하면 된다.
문제는 AdMob인데, 다운받은 SDK에서 `GoogleMobileAds.framework`랑 `UserMessagingPlafrom.framework` 두 파일만 가져와 프로젝트 안에 넣으면 된다.

프로젝트 -> General -> Frameworks, Libraries, and Embedded Contetn에서 `Do Not Embed`로 설정하고
프로젝트 -> Build Settings -> Other Linker Flags는 빈 칸으로 두었다.
`-ObjC` 사용하니까 Alamofire 쪽에서 어쩌구저쩌구 하면서 빌드가 안된다.

+)
`-ObjC` 안 넣으면 archive 할 때 죽음 ㅡㅡ 시발새끼야
그리고 왜 alamofire에서 에러 안 나냐 시발새끼야 ㅡㅡ