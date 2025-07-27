## 원격

### 목표: 적어두기
작성자: kkongnyang2 작성일: 2025-07-28

---
### > 호스트 원격 RDP

```
sudo apt install xrdp       # RDP 서버 프로그램(리눅스용)
sudo systemctl enable --now xrdp      # sudo systemctl disable <서비스>
sudo apt install xrdp xorgxrdp xfce4 -y   # xrdp가 xorg 세션을 만들수 있도록 하는 드라이버
gdm3 선택         # GNOME 안쓸거면 lightdm도 ok
echo "startxfce4" > ~/.xsession
sudo sed -i 's/^\(. \/etc\/X11\/Xsession\)/#\1/' /etc/xrdp/startwm.sh
echo "startxfce4" | sudo tee -a /etc/xrdp/startwm.sh
sudo systemctl restart xrdp
```
```
클라이언트 컴퓨터에서 win+r누르고 mstsc   # RDP 클라이언트 프로그램(윈도우용)
172.30.1.69:3389 입력
username: kkongnyang2 password: 4108
```
* RDP? 그래픽 명령을 네트워크로 보내서 클라이언트가 그린다

### > ssh 설명