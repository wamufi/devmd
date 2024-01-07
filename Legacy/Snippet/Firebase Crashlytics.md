---
boostnote:
  createdAt: '2020-11-26T12:02:36.558Z'
  updatedAt: '2020-11-26T12:05:59.722Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Firebase Crashlytics
  tags:
    - AppTools
    - Crashlytics
    - Firebase
    - Google
  linesHighlighted: []
  isStarred: false
  key: c2a87b98-7617-47bf-bcbb-b93fa9f5e115
  storage: d112f8ec1e85e056a09d
---

Firebase Crashlytics
---
앱의 비정상 종료를 찾아서 리포트 해주는 매우 좋은 기능인 것 같아 포스팅.

.xcodeproj의 Run Script
```
"${PODS_ROOT}/FirebaseCrashlytics/run"

if [ -d ${DWARF_DSYM_FOLDER_PATH}/${DWARF_DSYM_FILE_NAME} ]
then echo "+++++ DYMS FIND, send FirebaseCrashlytics dSYMs" "${PODS_ROOT}/FirebaseCrashlytics/upload-symbols" -gsp "${PROJECT_DIR}/GoogleService-Info.plist" -p ios "${DWARF_DSYM_FOLDER_PATH}/${DWARF_DSYM_FILE_NAME}"
else
echo "DYMS is not found, skip"
fi
```
추가. 그냥 첫줄만 넣으면 작동은 하나 콘솔에 dysm이 없다며 워닝이 나온다.

문서는 여기
[Get started with Firebase Crashlytics](https://firebase.google.com/docs/crashlytics/get-started)
