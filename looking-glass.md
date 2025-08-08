### looking glass

ivshmem 필요 램 계산
```
(가로 × 세로 × BPP × 2) / 1024 / 1024 + 10 → 가장 가까운 2^n MiB로 올림
BPP는 sdr과 hdr이 있는데 hdr로 해봤자 부과만 크니 sdr로 해주자. 그러면 2560x1440 기준 64MB라고 나와있다.
```

ivshmem-plain 만들기
```
# 우분투에서
$ virsh edit win10
<devices> 태그 산하에, 제일 마지막 즈음에 </devices>로 닫기 전에 넣어주자
    <!-- Looking-Glass shared-memory PCI 장치 -->
    <shmem name='looking-glass'>
      <model type='ivshmem-plain'/>
      <size unit='M'>64</size>
    </shmem>
$ echo "f /dev/shm/looking-glass 0660 $USER kvm -" | \
  sudo tee /etc/tmpfiles.d/10-looking-glass.conf
$ sudo systemd-tmpfiles --create
```
이렇게 하면 /dev/shm/looking-glass가 생기고 윈도우 장치 관리자에 system device에 PCI standard RAM Controller가 보임

looking glass host 설치
```
# 윈도우에서
https://looking-glass.io/downloads에서 official/stable version B7 windows host binary 다운로드
압축 풀고 looking glass host setup.exe 실행
설치 후 장치 관리자 들어가보면 PCI standard RAM Controller가 ivshmem device로 이름이 바껴있음
재부팅 뒤 Services.msc 에서 Looking Glass (host) = Running 확인.
```

looking glass client 설치
```
# 우분투에서
$ sudo apt install looking-glass-client
$ looking-glass-client -F
```


### 튜닝

libvirt xml 튜닝
```
$ virsh edit win10
1번 방식 선택
원본
    <memballoon model='virtio'>
      <address type='pci' domain='0x0000' bus='0x04' slot='0x00' function='0x0'/>
    </memballoon>
수정
    <memballoon model='none'/>
```

