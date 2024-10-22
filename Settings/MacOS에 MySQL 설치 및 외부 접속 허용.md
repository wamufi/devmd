---
title: MacOS에 MySQL 설치 및 외부 접속 허용
date: 2024-10-21
time: 14:49:47
tags:
  - database
  - mysql
---
설치 환경: MacOS Monterey 12.7 & Intel

### MySQL 설치 및 실행
#### 설치
`brew install mysql`

#### 실행
##### 부팅 시 자동 실행
`brew servies restart mysql`

##### 수동 실행
`mysql.server start`

##### 차이
[[MySQL][MAC] brew services start mysql와 mysql.server start 차이](https://sunghyun98.tistory.com/297)

### 외부 접속 허용
