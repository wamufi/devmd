---
title: SwiftUI 비동기 트리거 3종 비교
date:  2025-07-11
time:  01:49:16
tags:
  - swift
  - swiftui
---
|항목|`.task {}`|`.onAppear {}`|`onChange(of:) {}`|
|---|---|---|---|
|**트리거 시점**|View가 **body에 처음 표시될 때** 1회 실행(SwiftUI가 이 View를 "활성화"했을 때)|View가 **화면에 나타날 때마다** 실행|**특정 값이 변경될 때마다** 실행|
|**중복 실행**|일반적으로 1회 (View가 새로 만들어질 때)|화면 전환 후 돌아와도 다시 실행됨|바인딩 값이 바뀔 때마다 반복 실행|
|**비동기 지원**|✅ `await` 가능 (Swift 5.5+)|❌ 직접 `Task { await ... }`로 감싸야 함|❌ 직접 `Task { await ... }`로 감싸야 함|
|**사용 예**|네트워크 요청, 초기 데이터 로딩|뷰 전환 후 로직 실행, 타이밍 맞는 UI 동작|ViewModel이나 바인딩 값이 바뀔 때 반응 작업|
|**iOS 지원**|iOS 15+ (`.task(id:)`는 15.0부터)|iOS 13+|iOS 14+|

### 예제
#### 1. `.task`

swift

```swift
Text("Hello")
    .task {
	    await viewModel.loadData()
	}
```

> View가 **초기 로드**될 때 한 번만 실행됨.  
> View가 사라졌다 다시 나타나면 또 실행.

### 2. `.onAppear`

```swift
Text("Hello")
    .onAppear {
        Task {
            await viewModel.loadData()
        }
    }
```

> View가 **화면에 나타날 때마다 반복 실행**됨.  
> Navigation으로 왔다갔다 할 때 유용.

### 3. `onChange(of:)`

```swift
Text("Hello")
	.onChange(of: chapter) { newChapter in
	    Task {
	        await viewModel.loadData(newChapter)
	    }
	}
```

> `chapter` 값이 바뀔 때만 실행됨.  
> **상태나 바인딩 기반으로 동작 제어할 때 적합**.

### 언제 어떤 걸 써야 하나?

|상황|추천 방식|이유|
|---|---|---|
|View가 처음 표시될 때 한 번 실행|`.task`|간결하고 비동기 지원됨|
|View가 화면에 나타날 때마다 실행|`.onAppear`|사용자 이동에 반응|
|특정 값 바뀔 때 실행|`onChange(of:)`|값 기반 로직 처리 시 유용|
|SwiftUI 14 이하 지원|`.onAppear`|`.task`는 iOS 15+|