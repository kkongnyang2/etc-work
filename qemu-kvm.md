## qemu-kvm 옵션의 이해

만약 git으로 qemu 설치하고 싶으면
```
git clone https://gitlab.com/qemu-project/qemu.git
cd qemu
mkdir build
cd build
../configure --target-list=x86_64-softmmu --enable-kvm --disable-werror
ninja -j$(nproc)
ninja install
```

빌드 때는 어떤 타깃 아키텍처를 만들지, 어떤 GUI/디스플레이 백엔드/가속 라이브러리를 포함할지 결정

런타임 때는 VM 하드웨어 토폴로지, 가속(KVM), CPU 패스스루, 메모리, 펌웨어(OVMF), 디스크/네트워크/USB/그래픽 장치, VFIO 패스스루(vGPU 포함), 디버깅/관리(QMP,monitor) 등을 설정.

../configure 옵션

| 카테고리 | 대표 옵션 | 설명 |
|--------|---------|------|
|타깃 아키텍처|--target-list=x86_64-softmmu|필요한 에뮬레이션 대상만 빌드|
|설치 경로|--prefix=/usr/local|시스템 기본 패키지와 충돌 피하려면 별도 prefix|
|디버그|--enable-debug|문제 추적 유용|
|KVM 지원|--enable-kvm|보통 자동 감지되나 하는게 좋음|
|그래픽 백엔드|--enable-gtk,--enable-sdl,--enable-opengl|GUI창(gtk),레거시/대안(sdl),GL렌더링(virgl 등과 엮임)|
|오디오|--audio-drv-list=pa|오디오 백엔드 선택(필요 없으면 제거)|
|SPICE/remote|--enable-spice,--enable-usb-redir|원격 그래픽 및 USB 리다이렉션|
|VFIO 관련|거의 없음|시스템 라이브러리에 의존. 커널쪽 VFIO 모듈이 더 중요|
|스토리지|--enable-libiscsi|네트워크 스토리지 프로토콜 필요 시|
|LTO/최적화|--enable-lto|최적화 관련|


gtk? 가장 일반적인 선택. 창 크기 조절, 메뉴 제어, 마우스 포인터 통합 등등.
sdl? 경량 옵션. 창 크기 고정, 메뉴 없음, 마우스 포인터 중첩 불가. 키오스크 같은 고정 미니멀 환경

gl=on(opengl)? virtio-gpu를 사용할때 호스트로 전달받은 게스트 렌더링을 처리하기 위한 api.