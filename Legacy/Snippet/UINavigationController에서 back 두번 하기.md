---
boostnote:
  createdAt: '2019-02-18T09:46:18.690Z'
  updatedAt: '2019-03-06T03:07:42.626Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UINavigationController에서 back 두번 하기
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 6caa46c4-f6a4-4ccd-a8e2-d943e7f9f2fd
  storage: d112f8ec1e85e056a09d
---

UINavigationController에서 back 두번 하기
---
```objc
int viewControllerStackCount = self.navigationController.viewControllers.count;
[self.navigationController popToViewController:[self.navigationController.viewControllers objectAtIndex:viewControllerStackCount - 3] animated:true];
```

https://code.i-harness.com/ko-kr/q/2e3267

[김또치세상 :: 'popToRootViewControllerAnimated' 태그의 글 목록](https://kimddochi.tistory.com/tag/popToRootViewControllerAnimated)

최상단으로 보낼 때
---
```objc
[self.navigationController popToRootViewControllerAnimated:true];
```