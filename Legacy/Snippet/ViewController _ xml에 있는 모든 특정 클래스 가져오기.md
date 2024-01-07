---
boostnote:
  createdAt: '2019-12-09T06:22:20.108Z'
  updatedAt: '2019-12-10T05:35:03.281Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: ViewController / xml에 있는 모든 특정 클래스 가져오기
  tags:
    - Android
    - Java
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: 252603e9-c5bb-42ec-8176-e5e9c49cf961
  storage: d112f8ec1e85e056a09d
---

ViewController / xml에 있는 모든 특정 클래스 가져오기
---

iOS는 UILabel
```objc
@property (nonatomic, retain) NSMutableArray *labels;

- (void)test {
    _labels = [NSMutableArray new];
    [self getAllLabelsInView:self.view];

    for (UILabel *label in _labels) {
        label.textColor = textColor;
    }
}

- (void)getAllLabelsInView:(UIView *)view {
    for (id subview in view.subviews) {
        if ([subview isKindOfClass:[UILabel class]]) {
            UILabel *label = (UILabel *)subview;
            [_labels addObject:label];
        } else {
            [self getAllLabelsInView:subview];
        }
    }
}
```

Android는 ImageView
```java
private ArrayList<View> xmasImages = new ArrayList<>();

private void test() {
    LinearLayout contentMainLayout = findViewById(R.id.content_main_linear);
    getAllImageViewsInView(contentMainLayout);
}

private void getAllImageViewsInView(View view) {
    if (view instanceof ViewGroup) {
        for (int i = 0; i < ((ViewGroup) view).getChildCount(); i++) {
            if (((ViewGroup) view).getChildAt(i) instanceof ImageView) {
                xmasImages.add(((ViewGroup) view).getChildAt(i));
            } else {
                getAllImageViewsInView(((ViewGroup) view).getChildAt(i));
            }
        }
    }
}
```

https://stackoverflow.com/questions/25412606/find-uilabel-in-uiview-in-swift
[(Android) How can I find all ImageViews in my xml file? - Stack Overflow](https://stackoverflow.com/questions/32158877/android-how-can-i-find-all-imageviews-in-my-xml-file)