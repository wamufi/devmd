---
title: ffmpeg 명령어
date: 2025-01-22
time: 11:54:12
tags:
- command
---
## 자르기
### 파일 자르기
`ffmpeg -ss 01:04:50 -i input.webm -c copy outputcut.webm`

### 파일 두개로 자르기
`ffmpeg -i input.mp4 -c copy -map 0 -segment_time 00:02:00 -f segment output%03d.mp4`
  
## 인코딩
### webm to mp4 (av1 to h264)
`ffmpeg -fflags +genpts -i input.webm output.mp4`
`ffmpeg -i input.webm -c:v libx264 -c:a aac -strict -2 -movflags +faststart output.mp4`

### h264 인코딩
`ffmpeg -i input.mp4 -c:v libx264 -crf 0 output.mp4`
- `-c:v libx264`: `-codec:video`
- `-crf 0`: 압축률 0

##  기타
### m3u8 다운로드
`ffmpeg -i “asdf.m3u8” -c copy download.mp4`

### video audio merge
`ffmpeg -i video.mp4 -i audio.mp4 -c:v copy -c:a aac output.mp4`

### 코덱 정보

`ffmpeg -i input.mp4`