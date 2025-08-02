## 저장공간이 없을때 대처

작성자: kkongnyang2 작성일: 2025-08-01

---

### > 파티션 조정

윈도우 가서
window+x 눌러 디스크 관리 들어가 윈도우 파티션 우클 누르고 볼륨 축소
가능한 만큼 최대한 축소해 unallocated 파티션 만들기
ubuntu 22.04 desktop.iso 파일 다운
usb 꽂고 rufus 실행해 해당 파일로 gpt(uefi)로 만들기

부팅 중 f10으로 들어가 usb로 부팅
라이브 세션에서 터미널 들어가 sudo gparted로 파티션 재배치.
우분투 파티션 누르고 resize/move를 눌러 빈 공간 없이 채워주기
apply
라이브 세션 종료

다시 우분투로 돌아와서 용량 늘어난거 확인


### > 컴퓨터 정리

```
$ apt-mark showmanual     # apt를 통해 깐 패키지만 보여줌
$ sudo apt remove qemu-system-arm
$ snap list               # snap을 통해 깐 앱 보여줌
$ sudo snap remove rpi-imager
```