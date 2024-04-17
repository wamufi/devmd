---
title: Vue.js in Rocky Linux
date: 2024-04-17
time: 11:52:40
tags:
  - linux
  - vue
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

vue 프로젝트 외부 접속 허용 package.json
```
...
"scripts": {
  "dev": "vite --host"
}
...
```

**express는 필요 없을 것 같은데 확인이 필요**
express 설치
```
npm install express
```

express 설치 폴더에서 server.js
```
const express = require('express');
const app = express();

app.listen(5173, () => {
  console.log('server on');
})
```

server.js 실행
```
node server.js
```
**express는 필요 없을 것 같은데 확인이 필요**


