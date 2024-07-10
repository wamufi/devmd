---
title: Perforce with Unreal Engine
date: 2024-07-10
time: 11:31:02
tags:
- perforce
- unreal
---
# 설정
## root 디렉터리 설정
터미널에서 `p4d -r ~/Documents/Perforce`

## 유저 생성
서버의 P4Admin에서 File -> New... -> User..
standard로 생성

## 클라이언트에서 로컬 서버 접속 주소
시스템 환경 설정 -> 공유 -> 컴퓨터 이름 하단에 있는 'blahblah.local:1666'

# 프로젝트 연결
## depot 생성
서버의 P4Admin에서 File -> New... -> Depot...
Stream으로 생성

## stream 생성
서버의 P4V에서 depot 선택 -> Stream Graph -> New Stream...

## 추가
* depot은 프로젝트 별로 생성(하지 않아도 되지만)
* workspace는 전체 서버에서 공유
* workspace는 사용자_기기_프로젝트명 으로 생성

[How to Add/Create a Depot in Helix Core](https://www.youtube.com/watch?v=LITmtpuLrQQ)
[How to Create a Workspace](https://www.youtube.com/watch?v=R2vlwsoug4Y)
[언리얼 엔진에서 Perforce 소스 컨트롤 사용](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/using-perforce-as-source-control-for-unreal-engine)