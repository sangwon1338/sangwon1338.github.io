---
published: false
---




## 들어가며: 왜 도커(Docker)를 써야하나요??



### 0. 도커 설치 전

​	도커 설치하기 전에 기존 도커버전이 깔려 있다면 제거하고 시작해야 된다.

```shell
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```



#### 1. Docker 저장소 설정

```sh
$ sudo apt-get update
# 패키지 다운로드
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
# Docker GPG 키 추가
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# Docker GPG 키 등록 확인
$ sudo apt-key fingerprint 0EBFCD88
# Docker 저장소 등록
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```



### 2. Docker Engine 설치

```sh
$ sudo apt-get update
# Docker 최신버전 설치
$ sudo apt-get install docker-ce docker-ce-cli containerd.io 
# Docker 버전 확인
$ docker version
```

Output:

```sh
Client: Docker Engine - Community
 Version:           19.03.11
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        42e35e61f3
 Built:             Mon Jun  1 09:12:41 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.11
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       42e35e61f3
  Built:            Mon Jun  1 09:11:15 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```



### 3.  Docker compose 설치

```sh
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version
```



### 4. Docker의 일반사용자 권한 추가 & 제거

```sh
$ sudo usermod -aG docker $USER # 현재 사용자에게 권한주기
$ sudo deluser $USER docker # 현재 사용자의 docker 권한 제거
$ sudo service docker restart # 도커 시스템 재시작 하거나 컴퓨터 재시작
```



### 5. Docker 설치 확인

​	아래의 명령어를 실행 하였을때 Output 과 같이 나와야 한다.

​	에러가 난다면 제거후 다시 설치

```sh
$ docker run hello-world
```

Output:

```sh
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete
Digest: sha256:89b647c604b2a436fc3aa56ab1ec515c26b085ac0c15b0d105bc475be15738fb
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```



### 6. Docker Engine 제거

```sh
# 도커 패키지(docker-ce,cli) 제거
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io
# 이미지, 컨테이너, 볼륨, 사용자 지정 설정파일은 패지키 제거로 제거되지 않아서 별도로 제거
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```



