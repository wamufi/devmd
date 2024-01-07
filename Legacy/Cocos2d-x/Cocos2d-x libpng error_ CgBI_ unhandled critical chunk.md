---
boostnote:
  createdAt: '2021-10-27T05:00:15.080Z'
  updatedAt: '2021-10-27T05:05:40.955Z'
  type: MARKDOWN_NOTE
  folder: 4d5ef4df7aca689c0c48
  title: 'Cocos2d-x libpng error: CgBI: unhandled critical chunk'
  tags:
    - Cocos2d-x
    - Error
  linesHighlighted: []
  isStarred: false
  key: ca217966-1bdd-48fc-8d84-1199a6ad9bb4
  storage: d112f8ec1e85e056a09d
---

Cocos2d-x libpng error: CgBI: unhandled critical chunk
---
Sprite를 Resources/ 폴더에 추가하고
`auto sprite = Sprite::create("player.png");`
코드를 추가했을 때 nullptr 발생

Xcode에서 
`Build Settings -> Compress PNG Files -> No`
`Build Settings -> Remove Text Metadata from PNG Files -> No`
변경해주면 잘 읽어들인다

<https://discuss.cocos2d-x.org/t/libpng-error-cgbi-unhandled-critical-chunk/9276/9>
