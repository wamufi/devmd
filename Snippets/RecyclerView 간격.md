---
title: RecyclerView 간격
date: 2024-02-02
time: 11:18:59
tags:
  - android
  - kotlin
  - recyclerview
---
Horizontal
```kotlin
class HorizontalItemDecorator(private val width: Int) : RecyclerView.ItemDecoration() {  
	override fun getItemOffsets(outRect: Rect, view: View, parent: RecyclerView, state: RecyclerView.State) {  
		super.getItemOffsets(outRect, view, parent, state)  
		outRect.left = width
		outRect.right = width  
	}  
}
```

Vertical
```kotlin
	class VerticalItemDecorator(private val height: Int) : RecyclerView.ItemDecoration() {  
	override fun getItemOffsets(outRect: Rect, view: View, parent: RecyclerView, state: RecyclerView.State) {  
		super.getItemOffsets(outRect, view, parent, state)  
		outRect.top = height
		outRect.bottom = height  
	}  
}
```