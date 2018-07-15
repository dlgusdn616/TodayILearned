# Docker
## Tutorials

`docker run -it ubuntu:latest echo "hello, world"`

`docker run -it ubuntu:latest bash`
이 상태는 현재 우분투에 접속한 상태이다. `cat /etc/lsb-release`명령어로 우분투 터미널 내에 정보를 출력해보면 아래와 같은 메시지가 결과로 나온다.
```
root@c81de8a0fe10:/# cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04 LTS"
```



`docker run -it cents:latest bash` 명령어를 입력하면,  centos환경을 사용할 수 있다. 마찬가지로 `cat /etc/centos-release`명령어로 사용 중인 환경에 대해 알 수 있다.

현재 사용중인 컨테이너의 정보를 알고 싶다면 별도의 터미널을 열어서 `sudo docker ps -a`를 입력해주면 된다. 입력해주면 아래와 같은 결과가 나온다.

![docker_commands_01](/Users/wisecow/Documents/GitHub/TodayILearned/Docker/images/docker_commands_01.png)

이번엔 busybox를 이용해본다. busybox는 리눅스 커널 중 최소한의 기능들로만 구성된 가벼운 컨테이너라고 생각하면 된다. 다른 컨테이너에 비해 용량이 현저히 적다.
리눅스 배포판이 아닌 리눅스 커널을 실행할 수 있는 환경 + 툴들이 설치되어 있는 환경이다.

`busybody | head -n 1`명령어로 현재 busybox에 대한 정보를 알 수 있다. 결과는 아래와 같다.
`BusyBox v1.28.4 (2018-05-22 17:00:17 UTC) multi-call binary.`

## Basic Concepts

### 컨테이너

* 각각의 VM = 서로 다른 환경
  VM은 하드웨어의 가상화: 소프트웨어로 구현된 하드웨어

* 각각의 컨테이너 = 서로 다른 환경
  * OS에서 지원하는 기능을 사용
  * 격리된 환경에서 프로세스를 실행

![docker_architecture](/Users/wisecow/Documents/GitHub/TodayILearned/Docker/images/docker_architecture_01.jpg)

도커의 컨테이너는 `하드웨어의 가상화 없이 격리된 환경에서 실행되는 프로세스`이다 . 도커의 컨테이너는 프로세스라는 점이 중요하다.

**chroot**: 루트 디렉토리를 바꿔주는 역할을 하는 도구, 도커가 나오기 전에 사용되었던 것이다. 단 이 방법을 사용하기 위해서는 의존성을 확인한 후 각종 라이브러리를 가져와야 하는 불편한 작업이 수반된다.

예를 들어, 본래 /bin에 있었던 bash를 가져온다고 가정해보면 bash만 가져올 것이 아니라 `ldd bash`명령어로 필요한 의존성 라이브러리들을 파악한 후에 함께 복사해와야 실행이 가능하다.
실제로 컨테이너라는 건 **chroot**와 같은 격리기능을 좀 더 편리하게 제공해주는 것이라고 생각할 수 있다.

![docker_architecture](/Users/wisecow/Documents/GitHub/TodayILearned/Docker/images/docker_architecture_02.png)

도커의 경우 처음 lxc(linux container)를 이용해 격리를 했었고, 지금의 경우 libcontainer라는 라이브러리를 도커사에서 개발을 해서 사용하고 있다.

`docker run -p 3306:3306 --name mysql -d mysql` 명령어로 mysql을 실행시켜보자. 도커 안에서 실행되는 포트랑 밖에서의 포트랑 연결시켜주는 옵션이 `-p 3306:3306` 옵션이다. 

### 이미지

* 특정 프로세스를 실행하기 위한 환경
  * 계층화된 파일 시스템
  * 이미지는 파일들의 집합
  * 프로세스가 실행되는 환경도 결국 파일들의 집합

### 도커 서버와 클라이언트

* 리눅스 머신
  * 컨테이너를 네이티브하게 지원해준다. 컨테이너 = 호스트의 프로세스
  * 배포판에 따라 차이는 있지만 대부분 지원한다.
  * 실제 도커 컨테이너 배포에는 리눅스 머신을 사용한다.
* 맥
  * xhyve, 직접 컨트롤 할 수는 없지만 이 위에서 서버 및 컨테이너가 존재한다.
    * 클라이언트는 맥OS에 직접 설치되어 있다. 
    * xhyve는 맥OS의 경량화된 가상머신이다.
  * 호스트 머신과 자연스럽게 결합
    * 네트워크 / 볼륨 등
    * 호스트 머신처럼 사용 가능

