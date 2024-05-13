---
title: PostgreSQL 외부 접속 허용
date: 2024-04-17
time: 10:13:03
tags:
  - database
  - postgresql
---
사용자 이름: postgres
사용자 비밀번호: postgres

설치 환경: MacOS Monterey 12.7 & Intel, MacOS Sonoma 14.4 & M2

### PostgreSQL 설치 및 실행
#### 설치
`brew install postgresql`

### 실행
`brew services restart postgresql`

### 외부 접속 허용
#### postgresql.conf
M2: /opt/homebrew/var/postgresql@14/postgresql.conf
Intel: /usr/local/var/postgresql@14/postgresql.conf
```bash
# - Connection Settings -

#listen_addresses = 'localhost' # 기존 설정 주석 처리
listen_addresses = '*' # 새로 추가
```

#### pg_hba.conf
M2: /opt/homebrew/var/postgresql@14/pg_hba.conf
Intel: /usr/local/var/postgresql@14/pg_hba.conf
```bash
# IPv4 local connections: 
#host    all             all             127.0.0.1/32            trust # 기존 설정 주석 처리
host    all             all             0.0.0.0/0            trust # 새로 추가
```

#### 서비스 재시작
`brew services restart postgresql`

### PostgreSQL 사용자 추가
#### 접속
`psql postgres`

#### 사용자 추가
```
postgres=# CREATE ROLE postgres WITH LOGIN PASSWORD 'postgres';
CREATE ROLE
```

#### 권한 추가
```
postgres=# ALTER ROLE postgres CREATEDB CREATEROLE;
ALTER ROLE
```

#### 사용자 확인
```
postgres=# \du
```

#### 생성한 사용자로 접속
```
postgres=# \q
psql postgres -U postgres
```


https://reinvestment.tistory.com/66