---
title: Windows 10에 Tibero 설치 및 외부 접속
date: 2024-07-18
time: 11:17:11
tags:
  - database
  - tibero
---
SYS 비밀번호: tibero
데이터베이스 이름: tibero

설치 환경: Windows 10 & JDK 8, MacOS Sonoma 14.4 & M2
버전: 6

### Tibero 설치
#### 설치
MS > Windows (x86) 64-bit VM 다운로드 후 설치
구성모듈은 그냥 다 선택 (OLEDB는 64bit)

#### 데모 라이센스
Hostname은 콘솔창에서 hostname 입력 혹은 제어판 > 정보에서 컴퓨터 이름
ex. DESKTOP-!@#$
license.xml은 이메일에서 다운

#### Tibero 마법사
SYS Password: tibero
Create the database에서 fail이 나왔는데
Uninstall -> cmd > service.msc > tiber_tibero 삭제 -> 재기동 -> 재설치
해결

#### 실행
`tbsql sys/tibero`

### 외부 접속 (포트 개방)
#### 방화벽
고급 보안이 포함된 Windows Defender 방화벽 > 인바운드 규칙  > 새 규칙...
포트 -> TCP > 특정 로컬 포트 8629 -> 연결 허용 -> 도메인, 개인, 공용 -> 이름: tibero 접속 허용

#### ip 확인
`ipconfig`


### DBeaver에 Tibero 연결
#### 드라이버 생성
Database > Driver Manager > New
**Settings**
Driver Name: Tibero
Class Name: com.tmax.tibero.jdbc.TbDriver
URL Template: jdbc:tibero:thin:@{host}\[:{port}]:{database}
Default Port: 8629
Description: Tibero thin driver
**Libraries**
Add File > tibero6-jdbc.jar

#### 연결
Database > New Database Connection > Tibero
Host: 192.168.0.xx
Port: 8629
Database: tibero
Username: SYS
Password: tibero

### 계정 생성
#### sys 계정 접속
`tbsql sys/tibero`

#### 사용자 계정 생성
`create user testid identified by testpwd;`

#### 권한 부여
`grant connect to testid;`
`grant resource to testid;`

#### 생성한 계정으로 접속
`conn testid;`
`testpwd`

https://oioirang.tistory.com/78
https://velog.io/@dailylifecoding/Window-open-the-port-for-my-partner
https://erynn.tistory.com/28
https://blog.naver.com/korn123/30149644326

