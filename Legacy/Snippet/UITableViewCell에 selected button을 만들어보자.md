---
boostnote:
  createdAt: '2021-02-22T09:38:44.383Z'
  updatedAt: '2021-02-22T09:44:38.672Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: UITableViewCell에 selected button을 만들어보자
  tags:
    - iOS
    - Objective-C
    - UITableView
    - UITableViewCell
  linesHighlighted: []
  isStarred: false
  key: 2f187776-261d-4b5d-8ecd-15f0aeb3c2fa
  storage: d112f8ec1e85e056a09d
---

UITableViewCell에 selected button을 만들어보자
---
나는 버튼에 로티를 넣어서 그것은 감안하고

ViewController.h
```objc
@property (nonatomic, strong) NSMutableArray *favArray;
```

ViewController.m
```objc
@synthesize favArray;

- (void)viewDidLoad {
    favArray = [[NSMutableArray alloc] initWithCapacity:0]; // 즐겨찾기
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(nonnull NSIndexPath *)indexPath {
    MainTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"MainTableViewCell"];
    
    [cell.favButton addTarget:self action:@selector(favAction:) forControlEvents:UIControlEventTouchUpInside];
    cell.favButton.tag = indexPath.row;
    if ([favArray containsObject:[NSString stringWithFormat:@"%d", cell.favButton.tag]]) {
        cell.favAnimView.progress = 1;
    }
    
    return cell;
}

- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath {
    if ([cell isKindOfClass:[MainTableViewCell class]]) {
        MainTableViewCell *mainCell = (MainTableViewCell *)cell;
        if ([favArray containsObject:[NSString stringWithFormat:@"%d", indexPath.row]]) {
            mainCell.favAnimView.progress = 1;
        } else {
            mainCell.favAnimView.progress = 0;
        }
    }
}

// 즐겨찾기 버튼 액션
- (void)favAction:(UIButton *)sender {
    // Lottie 애니메이션
    for (UIView *view in sender.superview.subviews) {
        if ([view isKindOfClass:[AnimationView class]]) {
            AnimationView *animView = (AnimationView *)view;
            
            if (animView.progress >= 1) { // 노랑 -> 회색 즐겨찾기 제거
                [animView playToProgress:0 completion:^(BOOL succeeded) {
                    if (succeeded) {
                        NSLog(@"reverse success");
                    }
                }];
                [favArray removeObject:[NSString stringWithFormat:@"%d", sender.tag]];
            } else { // 회색 -> 노랑 즐겨찾기 추가
                [animView playWithCompletion:^(BOOL succeeded) {
                    if (succeeded) {
                        NSLog(@"success");
                    }
                }];
                [favArray addObject:[NSString stringWithFormat:@"%d", sender.tag]];
            }
        }
    }
}
```

MainTableViewCell.h
```objc
@property (nonatomic, weak) IBOutlet UIButton *favButton;
@property (nonatomic, retain) AnimationView *favAnimView;
```