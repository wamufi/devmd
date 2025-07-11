---
title: TabView에서 Toolbar 상세 화면 진입 시 UI 수정
date:  2025-07-05
time:  10:51:45
tags:
  - swift
  - swiftui
  - ios
---
![[Simulator Screen Recording - iPhone 16 - 2025-07-05 at 22.47.57 1.gif]]
이렇게 툴바가 위에 붙었다가 툭 내려오는 화면을

![[Simulator Screen Recording - iPhone 16 - 2025-07-05 at 22.50.35.gif]]
이렇게 수정해보자

```swift
struct TabToolView: View {
    var body: some View {
        TabView {
            ListView()
                .tabItem {
                    Image(systemName: "house")
                    Text("홈")
                }
            
            Text("검색 화면")
                .tabItem {
                    Image(systemName: "magnifyingglass")
                    Text("검색")
                }
            
            Text("설정 화면")
                .tabItem {
                    Image(systemName: "gear")
                    Text("설정")
                }
        }
    }
}

struct ListView: View {
    var body: some View {
        NavigationStack {
            List(1...20, id: \.self) { i in
                NavigationLink("아이템 \(i)") {
                    DetailView(item: i)
		                .toolbar(.hidden, for: .tabBar) // 탭바 숨기기

                }
            }
            .navigationTitle("리스트")
        }
    }
}

struct DetailView: View {
    @State private var isLoading = false

    let item: Int
    
    var body: some View {
        ZStack {
            if isLoading {
                ProgressView() // 로딩 중일 때 보여줄 화면
            } else {
                VStack {
                    Text("아이템 \(item) 상세")
                        .font(.title)
                    Spacer()
                }
                .toolbar {
                    ToolbarItemGroup(placement: .bottomBar) {
                        Button("저장") { }
                        Spacer()
                        Button("삭제") { }
                    }
                }
            }
        }
        .padding()
        .navigationTitle("아이템 \(item)")
//        .safeAreaInset(edge: .bottom) { // Toolbar 처럼 보이는 커스텀 뷰
//            HStack {
//                Button("Save") { }
//                Spacer()
//                Button("Delete") { }
//            }
//            .padding()
//            .padding(.horizontal, 8)
//            .background(.ultraThinMaterial)
//        }
        .onAppear {
            showProgressBriefly()
        }
    }
    
    // 가짜로 보여주는 0.1초짜리 Loading 화면
    private func showProgressBriefly() {
        // 이미 로딩 중이면 중복 실행 방지
        guard !isLoading else { return }
        isLoading = true
        // 0.1초 뒤에 false로
        Task {
            // 100밀리초 = 100_000_000 나노초
            try? await Task.sleep(nanoseconds: 100_000_000)
            // Main 스레드에서 상태 변경
            await MainActor.run {
                isLoading = false
            }
        }
    }
}
```


`if isLoading { } else{ }` 에서 else에 있는 뷰의 toolbar를 보여주기 전에 다른 뷰를 보여주고 그 뷰를 보여주면 toolbar가 자연스럽게 보인다.
딱히 할 게 없어서 0.1초 지연을 두었다.
이렇게 하면 자연스럽게 된다.

`NavigationStack { TabView { ... } }` 이런 방법도 있는 것 같긴 한데 잘 작동하지 않았던 걸로 기억한다.