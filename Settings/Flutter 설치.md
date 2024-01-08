---
title: Flutter 설치
date: 2024-01-08
time: 08:12:02
tags: 
- setting
- flutter
---
## 환경
macOS, IntelliJ IDEA CE, Homebrew

## 설치
### Flutter 설치
설치 명령어
`brew install flutter`

상태 확인 명령어
`flutter doctor`
### ios 설정
#### CocoaPods 설치
설치 명령어
`brew install cocoapods`

없어도 설치는 되는데 사용하게 되더라
#### xcode license
`sudo xcodebuild -license`
#### 기타
doctor에서 확인한 내용들 처리 해주면 된다. 나는 xcode를 계속 사용해서 그런지 `xcode-select`같은 내용은 나오지 않았다.

