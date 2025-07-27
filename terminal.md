## 터미널 명령어들

### 목표: 적어두기
작성자: kkongnyang2 작성일: 2025-07-28

---
### > 명령어 지속성 구분

영구 설정
/etc 에 적어두면 영구
/etc/default/grub 영구
systemctl enable 영구
virsh --config 영구
virsh edit
usermod -aG libvirt,kvm $USER 영구
* 다만 영구는 보통 재부팅해야 함

부팅동안(런타임)
/sys, /proc 에 echo 하면 이번 부팅 동안만
systemctl start 이번 부팅 동안만
systemctl stop 이번 부팅 동안만
modprobe vfio-pci

터미널/세션동안
/usr/bin에서 실행만 하면 터미널·세션 한정
newgrp libvirt 현재 셸에만
virsh --live 지금 실행 중인 vm에만


보통 파일은 영구, 명령은 휘발성이다


### > nano

선택 Ctrl+6
복사 Alt+6