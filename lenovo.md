## 레노버 노트북 개발환경 세팅

작성자: kkongnyang2 작성일: 2025-07-25

---

### 레노버 윈도우 설치

```
다른 컴퓨터로 윈도우 설치마법사 다운
실행해서 usb에 미디어 굽기
레노버 노트북에 꽂고 전원키기
바로 윈도우 설치 페이지 뜸
파티션 200GB만 윈도우용으로. 300GB는 우분투를 위해 남겨둠
키보드는 default (101 type1) - 이게 뭐냐? RALT는 한글키로, RCRTL은 한자키로 쓰는 키보드 배열.
네 키보드가 RALT 따로, 한글키 따로 되어 있지 않다면 이걸 선택.
네트워크 연결이 나오면 shift+f10 누르고 OOBE\BYPASSNRO 입력.
안되면 start ms-cxh:localonly
그러면 넘기기가 나올거임
완료
윈도우 잘 뜨면 window+R 누르고 diskmgmt.msc 들어가서 파티션 잘 되어있나 확인
windows powershell 들어가서 wmic csproduct로 시리얼 넘버 확인
```

### 레노버 드라이브 설치

```
이제 드라이버 설치
다시 다른 컴퓨터로 가라
QR 코드 찍고 노트북 페이지 들어가서 드라이버 - 수동 업데이트 - 무선랜(wifi) 드라이버 하나만 다운로드
usb에 옮기기
옮겨와서 꽂고 설치
설치하고 wifi 연결됐으면 레노버 vantage 설치
들어가서 자동 업데이트 돌리면 드라이버 알아서 깔아줌
키보드 백라이트나 점검이나 다 여기서 하면됨
만약 무선랜 드라이버가 이상하면 유선랜 연결하고서 무선랜 드라이버 다시 다운받아 설치
마지막으로 윈도우 정품인증 키 입력하면 끝
키 확인은 cmd에서 reg query “HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform” /v BackupProductKeyDefault
```

### 레노버 우분투 설치 (듀얼부팅)

```
우분투도 설치할거면
rufus 다운받기
우분투 24.04 desktop 미디어 다운받기
rufus 실행해서 usb에 미디어 굽기 (영구 공간 0, GPT, UEFI 추천)
포맷 형식은 FAT32(4gb 제한. uefi는 이걸로), NTFS(윈도우 전용), exFAT(안정성 취약), ext4(리눅스 전용) 중에
노트북 끄고 usb 꽂고 부팅
부팅 중에 f2든 f10이든 f12든 esc든 하나쯤 연타
그럼 바이오스로 넘어감
우선순위를 바꾸든 usb를 선택을 하든 클릭하면 우분투 설치 창이 뜰거임
try or install ubuntu 클릭 딴건 무시
설정 쭉 하고 간소화 버전으로 선택하고 그래픽 wifi 소프트웨어 설치 하는게 좋음
괜히 자동으로 맡겼다가 이상한데 설치할 수 있으니 수동 파티셔닝 선택
남겨둔 unallocated 영역에 설치
우분투가 잘 뜨면 gparted로 디스크 확인
```

### vscode 세팅

```
snap은 항상 여러가지 문제 발생. apt로 만들어주자
$ wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
$ sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
$ sudo apt update
$ sudo apt install code
extension에서 soft-era 설치
extension에서 power-mode 설치
power mode 설정 들어가서 허용 체크, 불꽃으로 선택, 콤보 위치는 editor, 카운팅 보이기, 카운터 사이즈 1.7, threshold 10, timeout 3, 타이머 보이기, 흔들기 허용, 흔들기 강도 1, explosions 허용, 커스텀 gif는 안함
github에 올리는 ssh 키 다시 만들어줌
$ ssh-keygen -t ed25519 -C "for-code"
```

### 키보드 세팅

```
24.04에서는 ibus-hangul이 기본 내장이라 패키지 설치x
$ ibus-setup 들어가 reference에 hangul 추가
설정 들어가 키보드 들어가 한국어(hangul) 추가 기존은 제거
이제 RALT를 HNGL로 설정해야 함
$ cd /usr/share/X11/xkb/keycodes
$ sudo cp evdev evdev.bak   # 백업 파일 남겨놓기. 되돌리려면 sudo cp evdev.bak evdev
$ sudo nano evdev           # 수정
원본:
<RALT> = 108;
<HNGL> = 130;
수정본:
<HNGL> = 108;
```