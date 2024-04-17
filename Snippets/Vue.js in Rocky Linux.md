---
title: Vue.js in Rocky Linux
date: 2024-04-17
time: 11:52:40
tags:
  - linux
  - setting
---
ssh 접속
```
ssh root(ID)@1.234.567.89 -p 3000(port number)
```

node.js 설치
```
curl -fsSL https://rpm.nodesource.com/setup_current.x | bash -
yum install -y nodejs
node -v
```

vue 프로젝트 생성
```
npm create vue@latest

cd <your-project-name>
npm install
npm run dev
```