---
title: jpg to png
date: 2024-10-04
time: 15:23:15
tags:
- kotlin
- android
---
```kotlin
fun convertCoverJpgToPng() {  
    val bufferedImage: BufferedImage = ImageIO.read(jpgFile.toFile())  
    ImageIO.write(bufferedImage, "png", createFilePath(jpgFile, createCoverFileName("png")).toFile())
}
```
