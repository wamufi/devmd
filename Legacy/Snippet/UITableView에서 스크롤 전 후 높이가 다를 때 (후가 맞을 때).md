---
boostnote:
  createdAt: '2020-10-15T04:50:05.012Z'
  updatedAt: '2020-10-15T04:52:57.727Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableView에서 스크롤 전 후 높이가 다를 때 (후가 맞을 때)
  tags:
    - Swift
    - UITableView
    - UITableViewCell
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 0e5dd721-6a87-4c65-a8fb-17d977594d73
  storage: d112f8ec1e85e056a09d
---

UITableView에서 스크롤 전 후 높이가 다를 때 (후가 맞을 때)
---
스크롤 전에는 잘려서 나왔는데 스크롤 후에는 맞게 잘 나왔을 때
```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "MissionStampTableViewCell") as! MissionStampTableViewCell
    cell.layoutIfNeeded() // 중요
    return cell
}
```

https://stackoverflow.com/questions/25971147/uitableview-dynamic-cell-heights-only-correct-after-some-scrolling
