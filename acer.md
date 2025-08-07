## 에이서 노트북 개발환경 세팅

작성자: kkongnyang2 작성일: 2025-08-04

---

### 레노버 윈도우 설치

```
다른 컴퓨터로 윈도우 설치마법사 다운
실행해서 FAT32로 포맷한 usb에 미디어 굽기
에이서 홈페이지 지원 phn16-72 검색해서 IRST 드라이버와 유선랜 드라이버 다운
압축 풀어 usb에 같이 넣어주기
에이서 노트북에 꽂고 전원키기
f2 연타해서 바이오스에서 부팅 순서 usb 1순위로

바로 윈도우 설치 페이지 뜸
마우스 연결
키 입력
파티션 설정 창에서 ssd 인식 안될거임
드라이버 로드 - 아까 usb에 넣어놓은 IRST VMD Controller 설치
ssd가 보이면 할당되지 않은 공간과 windriver 파티션만 뜰거임
윈도우 우분투 듀얼부팅을 위해 120000MB만 파티션 만들기
그럼 시스템/예약/주/할당x/windriver 이렇게 파티션이 될거임 
주에 윈도우 설치 시작
키보드는 default (101 type1) - 이게 뭐냐? RALT는 한글키로, RCRTL은 한자키로 쓰는 키보드 배열.
네 키보드가 RALT 따로, 한글키 따로 되어 있지 않다면 이걸 선택.
네트워크 없이 설치
완료
```

### 레노버 드라이브 설치

```
이제 드라이버 설치
아까 usb에 넣어놓은 유선랜 설치
랜선 꼽고 설정에 윈도우 업데이트 들어가서 전부 업데이트하면 알아서 맞는 드라이브 설치해줌
에이서 홈페이지 가서 Acer care center 프로그램 깔기
```

### 레노버 우분투 설치 (듀얼부팅)

```
rufus 다운받기
우분투 22.04 desktop 미디어 다운받기
rufus 실행해서 usb에 미디어 굽기 (영구 공간 0, GPT, UEFI 추천)
포맷 형식은 FAT32(대중적), NTFS(윈도우 전용), exFAT(안정성 취약), ext4(리눅스 전용) 중에 보통 FAT32
노트북 끄고 꽂고 전원키기
f2 연타해서 바이오스에서 부팅 순서 usb 1순위로
try or install ubuntu 클릭
설정 쭉 하고 간소화 버전으로 선택하고 드라이버는 두개(업데이트 다운, 써드파티 소프트웨어) 다 체크.
드라이버 업데이트는 아까 윈도우랑 비슷하고 써드파티는 third-party 외부 소프트웨어라 보면됨 필요한거 있으면 알아서 깔아줌 주로 nvidia
서드파티 드라이버 설치 중 UEFI Secure boot를 위해 MOK 비밀번호 설정 필요함 41080000
파티션은 수동 파티셔닝 선택
남겨둔 unallocated 영역을 ext4로 포맷하고 설치
우분투 설치가 완료되면 usb 빼고 엔터
그럼 재부팅 후 서드파티 드라이버 활성화 화면이 뜸
Enroll MOK, continue, yes 선택 후 아까 비밀번호 입력
그럼 설치가 되고 이제는 reboot를 택해주면 됨
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
github에 공개키 입력해주기
$ git config --global user.email "i.kkongnyang2@gmail.com"      # 사용자 설정
$ git clone git@github.com:kkongnyang2/etc-work.git
```

### 키보드 세팅

```
$ ibus-setup 들어가 reference에 hangul 추가
이제 RALT를 HNGL로 설정해야 함
$ cd /usr/share/X11/xkb/keycodes
$ sudo cp evdev evdev.bak   # 백업 파일 남겨놓기. 되돌리려면 sudo cp evdev.bak evdev
$ sudo nano evdev           # 수정
원본:
<RALT> = 108;
<HNGL> = 130;
수정본:
<HNGL> = 108;
설정 들어가 hangul 키보드 추가 후 토글키 설정, 기존 키보드 삭제
```