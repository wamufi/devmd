---
boostnote:
  createdAt: '2020-04-27T08:05:35.107Z'
  updatedAt: '2022-03-29T02:06:30.139Z'
  type: MARKDOWN_NOTE
  folder: ec2c5e1a46e8f816e4bb
  title: Decompile .apk
  tags:
    - Android
    - Decompile
  linesHighlighted: []
  isStarred: false
  key: b5a7efe7-2c20-450a-b741-dc078080463e
  storage: d112f8ec1e85e056a09d
---

Decompile .apk
===
1. download dex2jar
    - required to decompile .apk
    - [dex2jar download /| SourceForge.net](https://sourceforge.net/projects/dex2jar/)

2. download jd-gui
    - required to view decompiled .jar
    - [Java Decompiler](https://java-decompiler.github.io/)

3. run terminal
`sh d2j-dex2jar.sh app-release.apk`
    - if the terminal outputs './d2j_invoke.sh: Permission denied'
        `chmod 744 d2j_invoke.sh`
    - if successful, the terminal outputs
        `dex2jar app-release.apk -> ./app-release-dex2jar.jar`
        
4. run jd-gui and open app-release-dex2jar.jar

[[Mac] 안드로이드 APK 파일 Decomfile 하기 :: Mobile App Developer](https://chelsea-kbj.tistory.com/9)
[멀티덱스, 섣불리 적용하지 마세요! 프로가드로 메소드 카운트 줄이는 방법](https://www.androidhuman.com/lecture/proguard/2017/02/16/reduce-method-count-with-proguard/)
[APK 소스코드 분석하기 \| Popit](https://www.popit.kr/how-to-decompile-an-android-apps/)
[[기타] JD-GUI에서 No suitable Java version found on your system! 오류가 발생할 때](https://118k.tistory.com/1010)
