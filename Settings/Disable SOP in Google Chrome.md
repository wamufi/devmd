---
title: Disable SOP in Google Chrome
date: 2024-04-19
time: 11:45:13
tags:
  - setting
  - web
---
https://stackoverflow.com/questions/3102819/disable-same-origin-policy-in-chrome#comment56680542_3102819

macOS
```
open -a Google\ Chrome --args --disable-web-security --user-data-dir="tmp"
```

#### CORS
https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F