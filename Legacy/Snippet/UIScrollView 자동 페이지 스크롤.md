---
boostnote:
  createdAt: '2019-03-04T02:38:00.904Z'
  updatedAt: '2019-03-20T08:01:03.513Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UIScrollView 자동 페이지 스크롤
  tags:
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: b570554b-55f9-4253-be06-faa386146d33
  storage: d112f8ec1e85e056a09d
---

UIScrollView 자동 페이지 스크롤
---
```objc
- (void)drawScrollView {
    [[NSTimer scheduledTimerWithTimeInterval:5 target:self selector:@selector(scrollToBanner) userInfo:nil repeats:true] fire];
}

#pragma mark - UIScrollView Delegate
/**
 @brief
 스크롤뷰가 드래그 될 때 호출됩니다.
 
 @param scrollView 드래그 중인 스크롤 뷰
 */
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView {
    if ([scrollView isEqual:_bannerScrollView]){
        isPageControlBeingUsed = false;
    }
}

/**
 @brief
 스크롤이 종료 되었지만 가속으로 인하여 계속 스크롤되기 시작할 경우 호출됩니다.
 스크롤이 시작되는 시점에 1회 호출됩니다.
 
 @param scrollView 스크롤뷰
 */
- (void)scrollViewWillBeginDecelerating:(UIScrollView *)scrollView {
    if ([scrollView isEqual:_bannerScrollView]){
        isPageControlBeingUsed = false;
    }
}

/**
 @brief
 스크롤이 완전히 멈추게 되면 호출됩니다.
 pageController의 값을 정확히 설정하기 위해 카드 페이지가 변경됨을 알립니다.
 
 @param scrollView 스크롤뷰
 */
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView {
    if ([scrollView isEqual:_bannerScrollView]) {
        [self changeCurrentPage];
    }
}

/**
 @brief 스크롤뷰가 스크롤 중일때 호출됩니다.
 
 @param scrollView 스크롤 뷰
 */
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    if ([scrollView isEqual:_bannerScrollView] && isPageControlBeingUsed) {
        [self changeCurrentPage];
    }
}

/**
 @brief UIPageController의 값이 변화될 경우 호출됩니다.
 위치를 설정하고 현재 페이지가 이동되었는지를 판단합니다.
 현재 페이지에 따라 좌, 우 선택 버튼 사용여부를 설정합니다.
 */
- (void)changeCurrentPage {
    CGFloat scrollWidth = _bannerScrollView.frame.size.width;
    _bannerPageControl.currentPage = floor((_bannerScrollView.contentOffset.x - scrollWidth / totCnt) / scrollWidth) + 1;
    currentBannerPage = (int) _bannerPageControl.currentPage;
}

- (void)scrollToBanner {
    NSInteger page = currentBannerPage % totCnt;
    float scrollWidth = _bannerScrollView.frame.size.width;
    [_bannerScrollView setContentOffset:CGPointMake(page * scrollWidth, 0) animated:true];
    _bannerPageControl.currentPage = page;
    currentBannerPage++;
}
```