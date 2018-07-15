

# Docker

## 0. README

T아카데미의 도커 강의를 듣고 정리한 내용들입니다.

## 1. Tutorials

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



## 2. Basic Concepts

### 2.1 컨테이너

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

### 2.2 이미지

* 특정 프로세스를 실행하기 위한 환경
  * 계층화된 파일 시스템
  * 이미지는 파일들의 집합
  * 프로세스가 실행되는 환경도 결국 파일들의 집합

### 2.3 도커 서버와 클라이언트

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
* 맥OS와 가상머신
  * 일반적인 가상 머신
  * 컨테이너 = 가상 머신의 프로세스
  * 네트워크 / 볼륨 설정이 까다롭다.
  * 클라이언트는 환경변수를 참조해서 서버에 접속



### 2.4 컨테이너가 필요한 이유

* 컴퓨터의 환경은 보편적이지 않다.
  각각의 컴퓨터에서 mysql을 실행해본다고 생각해보자. 특정 하드웨어, OS, 특정 시점의 시스템 설정 등에 따라 전부 다르다. 통일할 수 있는 방법이 마땅히 존재하지 않는다.
* 상태 관리 및 서버관리는 정말 어렵다. 데스크톱도 똑같이 어렵다.
* 깨끗한 환경
  Dockerfile이란 깨끗환 환경(리눅스를 처음 설치했을 때와 같이)으로부터 애플리케이션 실행 환경까지의 최단 경로다. 
  최소한의 파일들만 모으는 작업이라고 생각하면 된다.
* 이미지 = 작동되는 상태이다. 이미지로 만들어 두고, 내가 실행시키고 싶은 프로세스를 정해주기만 하면 된다.
* 10명의 맥북이 있다면, 10개의 서로 다른 환경이 있지만, 하나의 이미지는 리눅스 환경에서 이 이미지를 사용할 때는 항상 같은 환경이라는 점을 보장할 수 있다.
* 초강력한 포터블 앱이다. 어떤 애플리케이션을 실행하기 위한 다양한 파일을 모아두고, 이것만 실행하면 된다.
* 재현성: 이미지로 만들면 어디든 공유가 가능하다. 여기서 되면 저기서도 된다.
* 오버헤드가 크지 않다. 만약 있다면, scale out 등으로 해결한다. 성능면에서 가상머신과 크게 문제 없다.



## 3. 실습하기

### 3.1 Agenda

* 컨테이너 실행하기 `run`
* 컨테이너 목록 확인하기 `ps`
* 컨테이너 중지하기 `stop`
* 컨테이너 제거하기 `rm`
* 컨테이너 로그보기 `logs`
* 이미지 목록 확인하기 `images`
* 이미지 다운로드하기 `pull`
* 이미지 삭제하기 `rmi`
* 네트워크 만들기 `network`
* 볼륨 마운트 `-v`
* Docker Compose



### 3.2 Mac or Windows

도커는 linux만 지원하기 때문에 MacOS와 Windows에 설치되는 Docker는 실제로 가상머신에 설치된다. MacOS는 `xhyve`를 사용하고 Windows는 `Hyper-V`를 사용한다.

설치가 제대로 되었는지 아래의 명령어와 결과로 확인해보자.

```
$ docker version
Client:
 Version:      18.03.1-ce
 API version:  1.37
 Go version:   go1.9.5
 Git commit:   9ee9f40
 Built:        Thu Apr 26 07:13:02 2018
 OS/Arch:      darwin/amd64
 Experimental: false
 Orchestrator: swarm

Server:
 Engine:
  Version:      18.03.1-ce
  API version:  1.37 (minimum version 1.12)
  Go version:   go1.9.5
  Git commit:   9ee9f40
  Built:        Thu Apr 26 07:22:38 2018
  OS/Arch:      linux/amd64
  Experimental: true
```



![docker_environment](/Users/wisecow/Documents/GitHub/TodayILearned/Docker/images/docker_environment_01.jpg)

### 3.3 컨테이너 실행하기

`docker run [OPTIONS] IMAGE[:TAG|@DIGEST][]COMMAND] [ARG...]`

|   옵션    |                          설명                          |
| :-------: | :----------------------------------------------------: |
|    -d     |       detached mode: 흔히 말하는 백그라운드 모드       |
|    -p     |        호스트와 컨테이너의 포트를 연결 (포워딩)        |
|    -v     |      호스트와 컨테이너의 디렉토리를 연결 (마운트)      |
|    -e     |          컨테이너 내에서 사용할 환경변수 설정          |
|  --name   |                   컨테이너 이름 설정                   |
|   --rm    |           프로세스 종료시 컨테이너 자동 제거           |
|    -it    | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
| --network |                     네트워크 연결                      |

