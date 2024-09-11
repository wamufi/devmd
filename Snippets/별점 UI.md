---
title: 별점 뷰
date: 2024-09-11
time: 11:08:56
tags:
- swiftui
- swift
---
```swift
struct ImageView: View {
    
    @State var rating = 0
    
    private let defaultImages = ["1.circle.fill", "2.circle.fill", "3.circle.fill", "4.circle.fill", "5.circle.fill"]
    private let highlightColors: [Color] = [.yellow, .red, .brown, .green, .cyan]
    
    var body: some View {
        HStack {
            ForEach(0..<5, id: \.self) { index in
                let highlightColor = highlightColors[index]
                
                Button {
                    rating = index + 1
                } label: {
                    Image(systemName: defaultImages[index])
                        .resizable()
                        .renderingMode(.template)
                        .foregroundColor(rating <= index ? .blue : highlightColor)
                        .shadow(color: highlightColor, radius: rating <= index ? 0 : 10)
                        .scaledToFit()
                }
            }
        }
        .padding()
    }
}

```