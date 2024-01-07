---
boostnote:
  createdAt: '2022-11-28T08:45:52.598Z'
  updatedAt: '2022-11-28T08:49:30.824Z'
  type: MARKDOWN_NOTE
  folder: ec2c5e1a46e8f816e4bb
  title: 'File system verify or repair failed. : (-69845)'
  tags: []
  linesHighlighted: []
  isStarred: false
  key: 5d80559b-eb42-4af4-9575-07111cf1a981
  storage: d112f8ec1e85e056a09d
---

File system verify or repair failed. : (-69845)
---
디스크 유틸리티 > 검사/복구 했을 때 `파일 시스템을 확인 또는 복구하는 데 실패했습니다. : (-69845)`에러 나왔을 때 해결 방법

재부팅 > command-R로 복구 모드 진입 > 터미널 실행(상단 바 도구? 아무튼 어딘가에 있음) > `diskutil list`로 마운트 할 위치 확인 > `fsck_apfs -y /dev/disk1s1` > 디스크 전체로 확인하라고 에러 떨어지면 > `fsck_apfs -y /dev/disk1` > PROFIT!

그리고 나서 재부팅 ㄱ


[Disk Utility Error - Apple Community](https://discussions.apple.com/thread/253724690)

[Disk Utility still can’t check and repair APFS volumes and containers – The Eclectic Light Company](https://eclecticlight.co/2021/11/19/disk-utility-still-cant-check-and-repair-apfs-volumes-and-containers/)
