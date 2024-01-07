---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UISwipeGestureRecognizer 사용 시에 좌우 Animation 효과
  tags:
    - Objective-C
    - iOS
  isStarred: false
  createdAt: '2017-12-15T05:36:21.674Z'
  updatedAt: '2019-03-06T03:09:16.499Z'
  linesHighlighted: []
  key: 4e748823f3b36c33aa8f
  storage: d112f8ec1e85e056a09d
---

UISwipeGestureRecognizer 사용 시에 좌우 Animation 효과
---
1. UISwipeGestureRecognizer 생성


```objc
UISwipeGestureRecognizer *swipeRight = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(moveToPrevious:)];
swipeRight.direction = UISwipeGestureRecognizerDirectionRight;
[self.view addGestureRecognizer:swipeRight];

UISwipeGestureRecognizer *swipeLeft = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(moveToNext:)];
swipeLeft.direction = UISwipeGestureRecognizerDirectionLeft;
[self.view addGestureRecognizer:swipeLeft];
```

2. Animation 생성


```objc
- (void)moveToPrevious:(UISwipeGestureRecognizer *)swipeGesture {
    CATransition *animation = [CATransition animation];
    [animation setDelegate:self];
    [animation setType:kCATransitionPush];
    [animation setSubtype:kCATransitionFromLeft];
    [animation setDuration:0.50];
    [animation setTimingFunction:[CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut]];
    [_barChartView.layer addAnimation:animation forKey:kCATransition];
}

- (void)moveToNext:(UISwipeGestureRecognizer *)swipeGesture {
    CATransition *animation = [CATransition animation];
    [animation setDelegate:self];
    [animation setType:kCATransitionPush];
    [animation setSubtype:kCATransitionFromRight];
    [animation setDuration:0.50];
    [animation setTimingFunction:[CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut]];
    [_barChartView.layer addAnimation:animation forKey:kCATransition];
}
```