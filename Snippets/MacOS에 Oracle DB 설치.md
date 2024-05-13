---
title: MacOS에 Oracle DB 설치
date: 2024-05-13
time: 14:41:29
tags:
  - database
  - oracle
---
컨테이너 이름: oracle
오라클 비밀번호: pass

설치 환경: MacOS Monterey 12.7 & Intel

### Oracle DB 설치 및 실행
#### Docker 설치
`brew install docker`

#### Colima 설치
`brew install colima`

#### Colima 실행
`colima start --memory 4 --arch x86_64`

#### Docker 실행
`docker run --name oracle -e ORACLE_PASSWORD=pass -p 1521:1521 -d gvenzl/oracle-xe`

#### Docker 컨테이너 조회
`docker ps`

### SQL plus
### SQL plus 실행
`docker exec -it oracle sqlplus`
user-name: system
password: pass

#### Oracle 접속
```bash
SQL> connect system/pass
Connected
```

#### hr 계정 언락
hr 계정은 Oracle 에서 기본으로 제공하는 계정. 실습용으로 사용하기에 적절한 인사(employees) 샘플 데이터 제공.
```bash
SQL> ALTER USER hr IDENTIFIED BY hr ACCOUNT unlock;
User altered.
SQL> commit;
```

#### hr 계정 접속
```bash
SQL> connect hr/hr
Connected
SQL> select * from employees;
```

#### SQL plus 종료
`SQL> exit`

### Docker 컨테이너
#### 시작
`docker start oracle`

#### 종료
`docker stop oracle`

#### 로그
`docker logs -f oracle`

### DBeaver
#### 연결
Create -> Connection
Host: 192.168.0.x
Port: 1521
Database: xe
Username: system
Password: pass

https://velog.io/@devsaza/M1-M2-Mac-OS%EC%97%90%EC%84%9C-Oracle-DB-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
https://medium.com/@hayeonkimmie/os%EC%97%90-oracle-sql-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-e6f862d1d3a8
https://davidev-wiki.tistory.com/entry/Mac%EC%97%90%EC%84%9C-Oracle11g-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-Docker-DBeaver