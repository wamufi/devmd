---
boostnote:
  createdAt: '2021-02-17T08:27:34.215Z'
  updatedAt: '2021-02-17T09:25:18.915Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: RecyclerView의 Adapter에서 Activity를 넘길 때 transition
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: 6045cde8-876d-4b76-a236-571f12c8b1d8
  storage: d112f8ec1e85e056a09d
---

RecyclerView의 Adapter에서 Activity를 넘길 때 transition
---
제목이 무슨 소리냐고? 나도 모르겠다...
Adapter를 클릭했을 때 Activity로의 전환에 애니메이션 효과를 추가해보자.

SDK 21 이상에서 작동한다.

bulid.gradle (app)
```
implementation 'androidx.transition:transition:1.4.0'
```


styles.xml
```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowContentTransitions">true</item>
    <item name="android:windowSharedElementsUseOverlay">false</item>
</style>
```

adapter_main.xml
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/recycler"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="10dp"
    android:transitionName="shared_element_container">
    
    ...
    
</LinearLayout>
```

activity_detail.xml
```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="?android:colorBackground"
    tools:context=".DetailActivity">
    
    ...
    
</CoordinatorLayout>
```

MainActivity.java
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    getWindow().requestFeature(Window.FEATURE_ACTIVITY_TRANSITIONS);
    setExitSharedElementCallback(new MaterialContainerTransformSharedElementCallback());
    getWindow().setSharedElementsUseOverlay(false);

    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
}
```

DetailActivity.java
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    getWindow().requestFeature(Window.FEATURE_ACTIVITY_TRANSITIONS);
    View sharedElementContainer = this.findViewById(android.R.id.content);
    sharedElementContainer.setTransitionName("shared_element_container");
    setEnterSharedElementCallback(new MaterialContainerTransformSharedElementCallback());
    getWindow().setSharedElementEnterTransition(new MaterialContainerTransform().addTarget(android.R.id.content).setDuration(1000L));
    getWindow().setSharedElementReturnTransition(new MaterialContainerTransform().addTarget(android.R.id.content).setDuration(900L));

    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_detail);
}
```

MainAdapter.java
```java
private Context mContext;
private MainActivity mainActivity;

@Override
public void onBindViewHolder(final MainBusAdapter.ViewHolder holder, int position) {
    holder.linear.setOnClickListener(new View.OnClickListener() {

        @Override
        public void onClick(View view) {
            Intent intent = new Intent(mContext, DetailActivity.class);
            ActivityOptionsCompat options = ActivityOptionsCompat.makeSceneTransitionAnimation(mainActivity, view, "shared_element_container");
            mainActivity.startActivity(intent, options.toBundle());
        }
    });
}
```

Adapter에서 `context.startActivity()`를 하면 적용이 안 된다.

[Material Design](https://material.io/develop/android/theming/motion)