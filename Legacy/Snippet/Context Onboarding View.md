---
boostnote:
  createdAt: '2021-03-31T08:07:54.092Z'
  updatedAt: '2021-12-15T08:09:56.757Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Context Onboarding View
  tags:
    - Objective-C
    - iOS
    - Java
    - Android
  linesHighlighted: []
  isStarred: false
  key: fd3f71c1-556c-4eb7-9ef2-6f29b241bdf9
  storage: d112f8ec1e85e056a09d
---

Context Onboarding View
---
까만 배경에 오버레이 되는 튜토리얼 만들기

각각 UITableView와 RecyclerView의 cell/adapter 안에 있는 아이템의 위치를 가져와 투명한 원(구멍)을 만들었다.

![Screenshot 2021-03-31 at 17.02.36.png](00ce49e0.png)

**iOS**
> viewDidAppear에 넣으면 UITableView 때문에 row 0, section 1의 정확한 위치와 높이를 못 가져옴
> row 0, section 0은 잘 가져옴

```objc
// 처음 튜토리얼
- (void)createOnboardingView {
    if ([onboardingLabelView superview]) {
        return;
    }
    
    // 까만 배경
    UIView *overlayView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, UIScreen.mainScreen.bounds.size.width, UIScreen.mainScreen.bounds.size.height)];
    overlayView.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.55];
    
    // 별표 버튼에 구멍
    CAShapeLayer *maskLayer = [[CAShapeLayer alloc] init];
    maskLayer.frame = overlayView.bounds;
    maskLayer.fillColor = [[UIColor blackColor] CGColor];
    
    CGFloat radius = 25.0;
    CGRect cellRectInTable = [mainTableView rectForRowAtIndexPath:[NSIndexPath indexPathForRow:0 inSection:0]];
    CGRect cellInWindow = [mainTableView convertRect:cellRectInTable toView:nil];
    CGRect cellRectInTable2 = [mainTableView rectForRowAtIndexPath:[NSIndexPath indexPathForRow:0 inSection:1]];
    
    int y = cellInWindow.origin.y + cellRectInTable2.origin.y + cellRectInTable2.size.height / 2;
    CGRect rect = CGRectMake(cellInWindow.size.width - radius * 2, y - radius, 2 * radius, 2 * radius);
    
    UIBezierPath *bezierPath = [UIBezierPath bezierPathWithRect:overlayView.bounds];
    maskLayer.fillRule = kCAFillRuleEvenOdd;
    [bezierPath appendPath:[UIBezierPath bezierPathWithOvalInRect:rect]];
    maskLayer.path = bezierPath.CGPath;
    overlayView.layer.mask = maskLayer;
    
    // 텍스트 뷰
    onboardingLabelView = [[UIView alloc] initWithFrame:CGRectMake(overlayView.frame.size.width - 200, rect.origin.y + radius * 1.8, 200, 80)];
    onboardingLabelView.backgroundColor = [UIColor whiteColor];
    onboardingLabelView.layer.cornerRadius = 5;
    onboardingLabelView.layer.masksToBounds = true;
    
    /*
    CGMutablePathRef path = CGPathCreateMutable();
    CGPathMoveToPoint(path, NULL, 0.0, 10);
    CGPathAddLineToPoint(path, NULL, 90, 10);
    CGPathAddLineToPoint(path, NULL, 100, 0);
    CGPathAddLineToPoint(path, NULL, 110, 10);
    CGPathAddLineToPoint(path, NULL, 200, 10);

    CAShapeLayer *shapeLayer = [CAShapeLayer layer];
    [shapeLayer setPath:path];
    [shapeLayer setFillColor:[[UIColor redColor] CGColor]];
    [shapeLayer setStrokeColor:[[UIColor blackColor] CGColor]];
//    [shapeLayer setBounds:labelView.frame];
    [shapeLayer setAnchorPoint:CGPointMake(0.0f, 0.0f)];
    [shapeLayer setPosition:CGPointMake(0.0f, 0.0f)];
    [[labelView layer] addSublayer:shapeLayer];

    CGPathRelease(path);
     */
    
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, onboardingLabelView.frame.size.width, onboardingLabelView.frame.size.height)];
    label.text = @"자주 이용하는 버스를 즐겨찾기에 등록하세요.";
    label.textColor = [UIColor blackColor];
    label.textAlignment = NSTextAlignmentCenter;
    label.numberOfLines = 0;
    [onboardingLabelView addSubview:label];
    
    // 한번 터치하면 사라지게 제스쳐 추가
    UITapGestureRecognizer *recognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(removeOnboardingView:)];
    [overlayView addGestureRecognizer:recognizer];
    
    // 적용
    [self.view addSubview:overlayView];
    [self.view addSubview:onboardingLabelView];
}

// 제거 제스쳐
- (void)removeOnboardingView:(UITapGestureRecognizer *)recognizer {
    [onboardingLabelView removeFromSuperview];
    [recognizer.view removeFromSuperview];
}
```

**Android**
> 내부 클래스로 OnboardingView를 만들어 사용
> 자주 쓰인다면 따로 빼는 것도 좋을 듯

```java
// 튜토리얼 추가
mRecyclerView.postDelayed(this::createOnboardingView, 50);

// 튜토리얼 생성
private void createOnboardingView() {
    RelativeLayout onboardingLayout = findViewById(R.id.main_onboarding);
    onboardingLayout.setOnTouchListener(new View.OnTouchListener() {
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            v.setVisibility(View.GONE);
            return true;
        }
    });

    // 까만 배경 + circle 위치
    MainBusAdapter.ViewHolder viewHolder = (MainBusAdapter.ViewHolder) mRecyclerView.findViewHolderForAdapterPosition(0);
    ImageButton favButton = viewHolder.mFavButton;

    int[] originalPos = new int[2];
    mRecyclerView.findViewHolderForAdapterPosition(0).itemView.getLocationInWindow(originalPos);

    // OnboardingView 생성
    OnboardingView onboardingView = new OnboardingView(mContext, originalPos[0] + favButton.getX() + favButton.getWidth() / 2, originalPos[1] + viewHolder.itemView.getHeight() / 2 - favButton.getWidth(), favButton.getWidth());

    // 텍스트
    TextView textView = new TextView(mContext);
    textView.setText("자주 이용하는 버스를\n즐겨찾기에 등록하세요.");
    textView.setTextAlignment(View.TEXT_ALIGNMENT_CENTER);
    textView.setBackgroundColor(Color.WHITE);
    textView.setPadding(30, 10, 30, 10);

    RelativeLayout.LayoutParams layoutParams = new RelativeLayout.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT);
    layoutParams.addRule(RelativeLayout.ALIGN_PARENT_END);
    textView.setLayoutParams(layoutParams);
    textView.setY(originalPos[1] + viewHolder.itemView.getHeight() / 2);

    // 까만 배경 뷰와 텍스트 추가
    onboardingLayout.addView(onboardingView);
    onboardingLayout.addView(textView);
}

// OnboardingView
private static class OnboardingView extends View {
    float circleX;
    float circleY;
    float radius;

    public OnboardingView(Context context, float circleX, float circleY, float radius) {
        super(context);

        this.circleX = circleX;
        this.circleY = circleY;
        this.radius = radius;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        Paint eraser = new Paint();
        eraser.setColor(0xFFFFFFFF);
        eraser.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.CLEAR));

        Bitmap b = Bitmap.createBitmap(canvas.getWidth(), canvas.getHeight(), Bitmap.Config.ARGB_8888);
        Canvas c = new Canvas(b);
        b.eraseColor(Color.TRANSPARENT);
        c.drawColor(Color.parseColor("#80000000"));
        c.drawCircle(circleX, circleY, radius, eraser);
        canvas.drawBitmap(b, 0, 0, null);
    }
}
```