---
boostnote:
  createdAt: '2022-10-06T01:58:11.599Z'
  updatedAt: '2022-10-06T02:02:50.317Z'
  type: MARKDOWN_NOTE
  folder: f9b4c2c416b7c9c01097
  title: x86_64 build error
  tags: []
  linesHighlighted: []
  isStarred: false
  key: d34fe0a6-867d-44e3-9907-866d553b21e9
  storage: d112f8ec1e85e056a09d
---

x86_64 build error
---
시뮬레이터는 x86_64를 쓰는데 아카이빙 하면서 에러난다고 x86_64를 라이브러리에서 지워버려서 에러 남

lipo -remove i386 DaumMap -o DaumMap
lipo -remove x86_64 DaumMap -o DaumMap
DaumMap.framework 폴더 안에서 실행 필요

이걸 실행해서 그렇게 됐습니다 실기기에서 하면 해결됨


[[Xcode] Xcode12에서 시뮬레이터 빌드 오류 원인 및 해결방법 – 토미의 개발노트 – iOS 개발관련 지식을 공유합니다.](https://jusung.github.io/Xcode12-Build-Error/)

m1은 여기 참조

[[Xcode 12] 인텔 맥북과 M1 맥북의 Architecture 차이점(x86_64와 Arm64)](https://chibest.tistory.com/72)
