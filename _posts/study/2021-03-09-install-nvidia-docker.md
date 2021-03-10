---
layout: post
title: '[Docker 기반의 딥러닝 환경 구축] 02. Nvidia docker 설치 (Ubuntu 18.04)'
subtitle: 설치
date: '2021-03-09 21:51:51 +0900'
categories: study
tags: docker
comments: true
published: true
related_posts:
  - category/_posts/study/2021-03-09-install-docker.md
---

# 개요

- 목차
    - [0.Nvidia-docekr 설치 전 Nvidia-driver 설치 필수](#0.Nvidia-docekr 설치 전 Nvidia-driver 설치 필수)
    - [1.Nvidia-docker 설치](#1.Nvidia-docker 설치)
    - [2.Docker에서 Cuda 환경으로 시작하기](#2.Docker에서 Cuda 환경으로 시작하기)
    - [Docker 실행 옵션](#Docker 실행 옵션)

### 0.Nvidia-docekr 설치 전 Nvidia-driver 설치 필수

우분투 18.04를 기준으로 Nivida-docker를 사용하기 위해서는 Nvidia-driver를 설치하여야 한다.

##### 그래픽 카드 확인

```sh
$ sudo lshw -C display
```

Output

```sh
*-display
       description: VGA compatible controller
       product: GP104 [GeForce GTX 1080]
       vendor: NVIDIA Corporation
       physical id: 0
       bus info: pci@0000:01:00.0
       version: a1
       width: 64 bits
       clock: 33MHz
       capabilities: pm msi pciexpress vga_controller bus_master cap_list rom
       configuration: driver=nvidia latency=0
```

다음 명령어로 추천드라이버를 확인한다.

```sh
$ ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001B80sv00001462sd00003367bc03sc00i00
vendor   : NVIDIA Corporation
model    : GP104 [GeForce GTX 1080]
driver   : nvidia-driver-410 - third-party free
driver   : nvidia-driver-460-server - distro non-free recommended
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-390 - distro non-free
driver   : nvidia-driver-415 - third-party free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-460 - third-party free
driver   : nvidia-driver-450 - third-party free
driver   : xserver-xorg-video-nouveau - distro free builtin
```



##### APT로 드라이버 설치

현재 그래픽카드와 호환이 되는 드라이버를 확인하고 apt로 설치가 가능하다.

먼저 다음 repository를 추가한다.

```sh
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
```

`apt-cache search`는 설치 가능한 드라이버 목록을 출력

```sh
$ apt-cache search nvidia | grep nvidia-driver-460
nvidia-driver-455 - Transitional package for nvidia-driver-460
nvidia-driver-460 - NVIDIA driver metapackage
nvidia-driver-460-server - NVIDIA Server Driver metapackage
```

이제 apt로 드라이버를 설치한다.

```sh
$ sudo apt-get install nvidia-driver-460
```

설치가 완료 되면 reboot 한다.

```sh
$ sudo reboot
```

만약 설치과정에서 기존에 설치된 프로그램들과 충돌이 발생하면 아래 명령어로 관련 프로그램을 삭제하고 다시 시도해보세요.

```sh
$ sudo apt --purge autoremove nvidia*
```



#### 1.Nvidia-docker 설치

```sh
$ curl -s -L <https://nvidia.github.io/nvidia-docker/gpgkey> | \\
    sudo apt-key add -
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L <https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list> | \\
    sudo tee /etc/apt/sources.list.d/nvidia-docker.list
$ sudo apt-get update
$ sudo apt-get install -y nvidia-docker2
```



### 2.Docker에서 Cuda 환경으로 시작하기

```sh
$ docker pull nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04
$ docker run --gpus all -it nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04 /bin/bash
$ nvidis-smi
```
![스크린샷 2021-03-10 오후 6 14 31](https://user-images.githubusercontent.com/70992303/110608524-ad431480-81cf-11eb-821c-b9b94c13279d.png)





### Docker 실행 옵션



|   옵션   |  설명  |
| :--: | :--: |
|   -d   |   detached mode 흔히 말하는 백그라운드 모드   |
|   -p   |   호스트와 컨테이너의 포트를 연결 (포워딩)   |
|   -v   |   호스트와 컨테이너의 디렉토리를 연결 (마운트)   |
|   -e   |   컨테이너 내에서 사용할 환경변수 설정   |
|   -name   | 컨테이너 이름 설정 |
|   -rm   | 프로세스 종료시 컨테이너 자동 제거 |
|   -it   | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
|   -lin   | 컨테이너 연결 [컨테이너명:별칭] |
|   --gpus   | 그래픽카드를 컨테이너에 연결하기 위한 옵션 |
