---
title: PostgreSQL 외부 접속 허용
date: 2024-04-17
time: 10:13:03
tags:
  - database
  - postgresql
---
macOS (M2)에서 brew로 설치 했을 때 기준

/opt/homebrew/var/postgresql@14/postgresql.conf 수정
```
# - Connection Settings -

#listen_addresses = 'localhost' # 기존 설정 주석 처리
listen_addresses = '*' # 새로 추가
```

/opt/homebrew/var/postgresql@14/pg_hba.conf 수정
```
# IPv4 local connections: 
#host    all             all             127.0.0.1/32            trust # 기존 설정 주석 처리
host    all             all             0.0.0.0/0            trust # 새로 추가
```

재시작
```
brew services restart postgresql
```