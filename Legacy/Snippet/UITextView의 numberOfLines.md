---
boostnote:
  createdAt: '2020-09-09T06:15:36.549Z'
  updatedAt: '2020-09-09T06:16:09.673Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITextView의 numberOfLines
  tags:
    - Swift
    - UITextView
    - iOS
  linesHighlighted: []
  isStarred: false
  key: fc88cc1c-0d1b-4e40-9fcd-29732a95ae7c
  storage: d112f8ec1e85e056a09d
---

UITextView의 numberOfLines
---
```swift
 func numberOfLines(textView: UITextView) -> Int {
    let layoutManager = textView.layoutManager
    let numberOfGlyphs = layoutManager.numberOfGlyphs
    var lineRange: NSRange = NSMakeRange(0, 1)
    var index = 0
    var numberOfLines = 0

    while index < numberOfGlyphs {
        layoutManager.lineFragmentRect(forGlyphAt: index, effectiveRange: &lineRange)
        index = NSMaxRange(lineRange)
        numberOfLines += 1
    }
    return numberOfLines
}
```