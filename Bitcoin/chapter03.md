# Chapter 3 Bitcoin Client

![Bitcoin core architecture](/Users/wisecow/Documents/GitHub/TodayILearned/Bitcoin/images/bitcoin-core-architecture.png "Bitcoin core architecture")

## Bitcoin Development Environment

* 우분투 16.04 server
* 개발 빌드 환경 셋업
    ```
    $ sudo apt install -y build-essential
    ```

## Compiling Bitcoin Core from the Source Code

### Selecting a Bitcoin Core Release

* github에서 클론
  ```
  ryan@Wolf:~$ git clone https://github.com/bitcoin/bitcoin.git
  ryan@Wolf:~$ cd bitcoin/
  ryan@Wolf:~$ git tag
  ryan@Wolf:~$ git checkout v0.15.0
  ryan@Wolf:~$ git status
  HEAD detached at v0.15.0
  nothing to commit, working directory clean
  ```

## Configuring the Bitcoin Core Build

* 필요한 환경 구성 파일 설치
  ```
  ryan@Wolf:~$ ./autogen.sh
  configuration failed, please install autoconf first
  
  ryan@Wolf:~$ sudo apt-get install -y autoconf
  
  ryan@Wolf:~$ ./autogen.sh
  
  ryan@Wolf:~$ ./autogen.sh
  configure.ac:28: installing 'build-aux/install-sh'
  configure.ac:28: installing 'build-aux/missing'
  Makefile.am:8: error: Libtool library used but 'LIBTOOL' is undefined
  Makefile.am:8:   The usual way to define 'LIBTOOL' is to add 'LT_INIT'
  Makefile.am:8:   to 'configure.ac' and run 'aclocal' and 'autoconf' again.
  Makefile.am:8:   If 'LT_INIT' is in 'configure.ac', make sure
  Makefile.am:8:   its definition is in aclocal's search path.
  Makefile.am: installing 'build-aux/depcomp'
  /usr/share/automake-1.15/am/depend2.am: error: am__fastdepCXX does not appear in  AM_CONDITIONAL
  /usr/share/automake-1.15/am/depend2.am:   The usual way to define 'am__fastdepCXX' is to  add 'AC_PROG_CXX'
  /usr/share/automake-1.15/am/depend2.am:   to 'configure.ac' and run 'aclocal' and   'autoconf' again
  /usr/share/automake-1.15/am/depend2.am: error: AMDEP does not appear in AM_CONDITIONAL
  /usr/share/automake-1.15/am/depend2.am:   The usual way to define 'AMDEP' is to add one   of the compiler tests
  /usr/share/automake-1.15/am/depend2.am:     AC_PROG_CC, AC_PROG_CXX, AC_PROG_OBJC,  AC_PROG_OBJCXX,
  /usr/share/automake-1.15/am/depend2.am:     AM_PROG_AS, AM_PROG_GCJ, AM_PROG_UPC
  /usr/share/automake-1.15/am/depend2.am:   to 'configure.ac' and run 'aclocal' and   'autoconf' again
  Makefile.am: error: C++ source seen but 'CXX' is undefined
  Makefile.am:   The usual way to define 'CXX' is to add 'AC_PROG_CXX'
  Makefile.am:   to 'configure.ac' and run 'autoconf' again.
  parallel-tests: installing 'build-aux/test-driver'
  autoreconf: automake failed with exit status: 1
  ```


  ryan@Wolf:~$ sudo apt-get install -y libtool


  ryan@Wolf:~$ ./autogen.sh
  pyenv: libtoolize: command not found

  The `libtoolize' command exists in these Python versions:
    anaconda3-5.2.0

  (anaconda3-5.2.0) ryan@Wolf:~$ pyenv global anaconda3-5.2.0

  (anaconda3-5.2.0) ryan@Wolf:~$ ./autogen.sh
  libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, 'build-aux'.
  libtoolize: copying file 'build-aux/ltmain.sh'
  libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'build-aux/m4'.
  libtoolize: copying file 'build-aux/m4/libtool.m4'
  libtoolize: copying file 'build-aux/m4/ltoptions.m4'
  libtoolize: copying file 'build-aux/m4/ltsugar.m4'
  libtoolize: copying file 'build-aux/m4/ltversion.m4'
  libtoolize: copying file 'build-aux/m4/lt~obsolete.m4'
  configure.ac:45: installing 'build-aux/compile'
  configure.ac:45: installing 'build-aux/config.guess'
  configure.ac:45: installing 'build-aux/config.sub'
  configure.ac:28: installing 'build-aux/missing'
  Makefile.am: installing 'build-aux/depcomp'
  libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, 'build-aux'.
  libtoolize: copying file 'build-aux/ltmain.sh'
  libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'build-aux/m4'.
  libtoolize: copying file 'build-aux/m4/libtool.m4'
  libtoolize: copying file 'build-aux/m4/ltoptions.m4'
  libtoolize: copying file 'build-aux/m4/ltsugar.m4'
  libtoolize: copying file 'build-aux/m4/ltversion.m4'
  libtoolize: copying file 'build-aux/m4/lt~obsolete.m4'
  configure.ac:10: installing 'build-aux/compile'
  configure.ac:5: installing 'build-aux/config.guess'
  configure.ac:5: installing 'build-aux/config.sub'
  configure.ac:9: installing 'build-aux/install-sh'
  configure.ac:9: installing 'build-aux/missing'
  Makefile.am: installing 'build-aux/depcomp'
  parallel-tests: installing 'build-aux/test-driver'
  libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, 'build-aux'.
  libtoolize: copying file 'build-aux/ltmain.sh'
  libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'build-aux/m4'.
  libtoolize: copying file 'build-aux/m4/libtool.m4'
  libtoolize: copying file 'build-aux/m4/ltoptions.m4'
  libtoolize: copying file 'build-aux/m4/ltsugar.m4'
  libtoolize: copying file 'build-aux/m4/ltversion.m4'
  libtoolize: copying file 'build-aux/m4/lt~obsolete.m4'
  configure.ac:78: installing 'build-aux/compile'
  configure.ac:28: installing 'build-aux/config.guess'
  configure.ac:28: installing 'build-aux/config.sub'
  configure.ac:38: installing 'build-aux/install-sh'
  configure.ac:38: installing 'build-aux/missing'
  Makefile.am:12: warning: user variable 'GZIP_ENV' defined here ...
  /usr/share/automake-1.15/am/distdir.am: ... overrides Automake variable 'GZIP_ENV'  defined here
  src/Makefile.am: installing 'build-aux/depcomp'
  src/Makefile.am:497: warning: user target '.mm.o' defined here ...
  /usr/share/automake-1.15/am/depend2.am: ... overrides Automake target '.mm.o' defined   here
  parallel-tests: installing 'build-aux/test-driver'
  ```

  * makefile 생성
  ```
    (anaconda3-5.2.0) ryan@Wolf:~$ ./configure
    ...
    configure: error: libdb_cxx headers missing, Bitcoin Core requires this library for     wallet functionality (--disable-wallet to disable wallet functionality)
    
    (anaconda3-5.2.0) ryan@Wolf:~$ sudo add-apt-repository ppa:bitcoin/bitcoin
    
    (anaconda3-5.2.0) ryan@Wolf:~$ sudo apt-get update
    
    (anaconda3-5.2.0) ryan@Wolf:~$ sudo apt-get install libdb4.8-dev libdb4.8++-dev
    
    (anaconda3-5.2.0) ryan@Wolf:~$ ./configure
    ...
    ...
    checking for mismatched boost c++11 scoped enums... ok
    configure: error: No working boost sleep implementation found.
    ```

  * Bitcoin core에 dependency한 라이브러리의 모음 필요
    ```
    (anaconda3-5.2.0) ryan@Wolf:~$ sudo apt-get install -y build-essential autoconf     libssl-dev libboost-dev libboost-chrono-dev libboost-filesystem-dev     libboost-program-options-dev libboost-system-dev libboost-test-dev libboost-thread-dev

    (anaconda3-5.2.0) ryan@Wolf:~$ ./configure
    ...
    ...
    configure: error: libevent not found.

    (anaconda3-5.2.0) ryan@Wolf:~$ sudo apt-get install -y libevent-dev

    (anaconda3-5.2.0) ryan@Wolf:~$ ./configure
    ...
    ...

    Options used to compile and link:
      with wallet   = yes
      with gui / qt = no
      with zmq      = no
      with test     = yes
      with bench    = yes
      with upnp     = auto
      debug enabled = no
      werror        = no

      target os     = linux
      build os      =

      CC            = gcc
      CFLAGS        = -g -O2
      CPPFLAGS      =  -DHAVE_BUILD_INFO -D__STDC_FORMAT_MACROS
      CXX           = g++ -std=c++11
      CXXFLAGS      = -g -O2 -Wall -Wextra -Wformat -Wvla -Wformat-security     -Wno-unused-parameter
      LDFLAGS       =
      ARFLAGS       = cr
    ```

### Building the Bitcoin Core Executables

  * 컴파일 그리고 빌드
    ```
    (anaconda3-5.2.0) ryan@Wolf:~$ make && make install

    (anaconda3-5.2.0) ryan@Wolf:~$ which bitcoind
    ...(없음)

    (anaconda3-5.2.0) ryan@Wolf:~$ make clean

    (anaconda3-5.2.0) ryan@Wolf:~$ make -j2

    (anaconda3-5.2.0) ryan@Wolf:~$ make check
    PASS: test/unitester
    ============================================================================
    Testsuite summary for univalue 1.0.2
    ============================================================================
    # TOTAL: 1
    # PASS:  1
    # SKIP:  0
    # XFAIL: 0
    # FAIL:  0
    # XPASS: 0
    # ERROR: 0
    ============================================================================

    (anaconda3-5.2.0) ryan@Wolf:~$ sudo make install

    (anaconda3-5.2.0) ryan@Wolf:~$ which bitcoind
    /usr/local/bin/bitcoind

    (anaconda3-5.2.0) ryan@Wolf:~$ which bitcoin-cli
    /usr/local/bin/bitcoin-cli

    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoind --help
    Bitcoin Core Daemon version v0.15.0

    Usage:
      bitcoind [options]                     Start Bitcoin Core Daemon

    Options:

      -?
           Print this help message and exit

      -version
           Print version and exit

      -alertnotify=<cmd>
           Execute command when a relevant alert is received or we see a really
           long fork (%s in cmd is replaced by message)

      -blocknotify=<cmd>
           Execute command when the best block changes (%s in cmd is replaced by
           block hash)
    ```

## Running a Bitcoin Core Node

* Bitcoin P2P는 Node로 구성
* 자원봉사자 실행 혹은 비트코인 응용프로그램을 만드는 일부 비지니스에 실행
* 실행 중인 비트코인 네트워크에서는 로컬 복사본에 자체 시스템에 확인 됨
* 노드를 실행하려면 모든 비트 코인 트랜잭션을 처리 할 수있는 충분한 리소스가 있는 시스템 필요
  * 특히 디스크 공간과 RAM이 필요
    * 모든 트랜잭션을 인덱싱할지 여부
    * 블록 체인의 전체 복사본을 유지할지 여부
  * 2018 년 초 현재 풀 인덱스 노드는 2GB의 RAM과 최소 160GB의 디스크 공간이 필요
    * 현재 사이즈 참조: [Blockchain.info](https://blockchain.info/charts/blocks-size)
  * Bitcoin의  노드는 비트 코인 트랜잭션이 담긴 블록을 전송 및 수신하여 인터넷 대역폭 소비
    * 인터넷 연결이 제한되어 있거나 데이터 용량이 낮은 네트워크에서 실행하면 안됨

* 여러 다양한 노드가 있음
  * 라즈베리파이
  * 클라우드의 VPC
  
* 왜 노드를 만들려고 하는가? (실행하고 싶은가?)
  * Bitcoin 소프트웨어를 개발하고 비트코인 네트워크 및 블록체인 API 접근을 위해 Bitcoin 노드 의존해야 하는경우
  * 비트 코인의 합의 규칙에 따라 트랜잭션의 유효성을 검사해야하는 응용 프로그램을 개발하는 경우
    * 일반적으로 비트 코인 소프트웨어 회사는 여러 노드를 운영
  * 비트코인 지원자
    * 노드를 생성하면 네트워크가 강력해짐
    * 더 많은 월렛, 더 많은 사용자, 더 많은 트랜잭션 처리
  * 거래를 처리하고 유효성을 확인하기 위해 3자에 의존하기 원하지 않은 경우

### Configuring the Bitcoin Core Node

* 비트코인 코어는 실행할 때마다 Configuration 파일을 찾음
* 비트코인 Configuration 파일을 위치를 알려면
  * 실제로 아래 명령어를 실행하면 bitcoin.conf 파일은 존재하지 않음 그래서 그냥 만듦
    ```
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoind -printtoconsole

    2018-07-08 04:47:05 Bitcoin version v0.15.0
    2018-07-08 04:47:05 InitParameterInteraction: parameter interaction:    -whitelistforcerelay=1 -> setting -whitelistrelay=1
    2018-07-08 04:47:05 Assuming ancestors of block     0000000000000000003b9ce759c2a087d52abc4266f8f4ebd6d768b89defa50a have valid signatures.
    2018-07-08 04:47:05 Using the 'standard' SHA256 implementation
    2018-07-08 04:47:05 Using RdRand as an additional entropy source
    2018-07-08 04:47:05 Default data directory /home/ryan/.bitcoin
    2018-07-08 04:47:05 Using data directory /home/ryan/.bitcoin
    2018-07-08 04:47:05 Using config file /home/ryan/.bitcoin/bitcoin.conf
    ```
  * Ctrl + C 하면 명령어로부터 exit
* 비트코인 코어는 100개가 넘는 옵션을 제공
  * 노드의 특성에 맞게 구성함
    ```bash
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoind --help
    ```
* 중요한 주요 옵션
  * -alertnotify
    * 지정된 명령 또는 스크립트를 실행하여 노드의 소유자 (일반적으로 전자 메일)에 긴급 경보 전송
  * -conf
    * config 파일 위치를 변경해서 사용할 수 있음
  * -datadir
    * 모든 Blockchain 데이터를 저장할 디렉토리와 파일 시스템을 선택
    * 기본적으로 .bitcoin 서브 디렉토리
    * 여유 공간이 있는 쪽으로 위치하는 것이 좋음
  * -prune
    * 이전 블록을 삭제하여 디스크 공간을 절약할 수 있음
  * -txindex
    * 모든 트랜잭션의 인덱스를 유지.
    * 이 옵션을 사용하면 getrawtransaction 으로 모든 Tx에 접근 가능
    * Full-index node 라고 함
    * Default 값은 0
  * -dbcache
  * -maxconnections
    * 인터넷 사용량이 제한된 상황에서 연결된 node를 제한하는 옵셥
  * -maxmempool
    * 트랜재션의 메모리
    * 기본 MB
    * Default 값은 300MB
  * -maxreceivebuffer/maxsendbuffer
    * 커넥션마다 최대 수신 버퍼 사이즈: 단위 byte, Default: 5000 Bytes
    * 커넥션마다 최대 전송 버퍼 사이즈: 단위 byte, Default: 1000 Bytes
  * -minrelaytxfee
    * 중계할 최소 수수료 비. 
    * 이 값의 아래는 트랜잭션 비표준으로 다뤄지고 트랜잭션 풀에서 거절되거나 중계되지 않음
    * 수수료는 BTC/kb
    * Default: 0.00001

* 비트코인 코어는 사용자 월렛과 관련된 트랜잭션만을 포함한 데이터베이스를 빌드한다.
  * getrawtransaction 명령어 등을 트랜잭션 디코딩 하고 싶다면,
    * configuration 파일에 txindex=1 설정하고
        ```
        $ bitcoind -reindex
        ```
* Full-index 노드의 Configuration 예
    ```bash
    alertnotify=myemailscript.sh "Alert: %s"
    datadir=/lotsofspace/bitcoin
    txindex=1
    ```
* 리소스 제한적인 환경에서 Configuration 예
    ```bash
    alertnotify=myemailscript.sh "Alert: %s"
    maxconnections=15
    prune=5000
    dbcache=150
    maxmempool=150
    maxreceivebuffer=2500
    maxsendbuffer=500
    ```
* [Config 파일 자동 샘플 설정 사이트](https://jlopp.github.io/bitcoin-core-config-generator/)

* Configuration 설정 후, 실행
  * 비트코인 코어 데몬 실행
    ```
    anaconda3-5.2.0) ryan@Wolf:~$ bitcoind -printtoconsole
    ```
  * 다른 콘솔에서 블록 확인
    ```
    (anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli getblockchaininfo
    {
      "chain": "main",
      "blocks": 0,
      "headers": 134000,
      "bestblockhash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
      "difficulty": 1,
      "mediantime": 1231006505,
      "verificationprogress": 2.990740490091765e-09,
      "chainwork": "0000000000000000000000000000000000000000000000000000000100010001",
      "pruned": true,
      "softforks": [
        {
          "id": "bip34",
          "version": 2,
          "reject": {
            "status": false
          }
        },
        {
          "id": "bip66",
          "version": 3,
          "reject": {
            "status": false
          }
        },
        {
          "id": "bip65",
          "version": 4,
          "reject": {
            "status": false
          }
        }
      ],
      "bip9_softforks": {
        "csv": {
          "status": "defined",
          "startTime": 1462060800,
          "timeout": 1493596800,
          "since": 0
        },
        "segwit": {
          "status": "defined",
          "startTime": 1479168000,
          "timeout": 1510704000,
          "since": 0
        }
      },
      "pruneheight": 0
    }
    ```

## Bitcoin Core Application Programming Interface (API)

* 비트코인 코어는 bitcoin-cli 커맨드를 아용하여 접근할 수 있는 JSON-RPC 인터페이스 구현
* 이 명령어는 API를 통해서 프로그래머블하게 할 수 있게 해줌

    ```
    (anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli help
    error: Could not locate RPC credentials. No authentication cookie   could be found, and no rpcpassword is set in the configuration file   (/home/ryan/.bitcoin/bitcoin.conf)
    (anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli help
    error: couldn't connect to server: unknown (code -1)
    (make sure server is running and you are connecting to the correct  RPC port)
    (anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli help
    error code: -28
    error message:
    Loading block index...
    (anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli help
    == Blockchain ==
    getbestblockhash
    getblock "blockhash" ( verbosity )
    getblockchaininfo
    getblockcount
    getblockhash height
    getblockheader "hash" ( verbose )
    getchaintips
    getchaintxstats ( nblocks blockhash )
    getdifficulty
    getmempoolancestors txid (verbose)
    getmempooldescendants txid (verbose)
    getmempoolentry txid
    getmempoolinfo
    getrawmempool ( verbose )
    gettxout "txid" n ( include_mempool )
    gettxoutproof ["txid",...] ( blockhash )
    gettxoutsetinfo
    preciousblock "blockhash"
    pruneblockchain
    verifychain ( checklevel nblocks )
    verifytxoutproof "proof"
    ...
    ```

### Getting Information on the Bitcoin Core Client Status

```
// JSON 타입으로 리턴
(anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli getinfo
{
"deprecation-warning": "WARNING: getinfo is deprecated and will be fully removed in 0.16. Projects should transition to using getblockchaininfo, getnetworkinfo, and getwalletinfo before upgrading to 0.16",
"version": 150000,
"protocolversion": 70015,
"walletversion": 139900,
"balance": 0.00000000,
"blocks": 88998,
"timeoffset": 0,
"connections": 6,
"proxy": "",
"difficulty": 3091.736890411797,
"testnet": false,
"keypoololdest": 1531025132,
"keypoolsize": 2000,
"paytxfee": 0.00000000,
"relayfee": 0.00001000,
"errors": ""
}
```

```
(anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli help getblockhash
getblockhash height
Returns hash of block in best-block-chain at height provided.
Arguments:
1. height         (numeric, required) The height index
Result:
"hash"         (string) The block hash
Examples:
> bitcoin-cli getblockhash 1000
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockhash", "params": [1000] }' -H   'content-type: text/plain;' http://127.0.0.1:8332/
```

```
// 1000 번째 블록
(anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli getblockhash 1000
00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09
```

```
(anaconda3-5.2.0) ryan@Wolf:~/.bitcoin$ bitcoin-cli getnetworkinfo
{
  "version": 150000,
  "subversion": "/Satoshi:0.15.0/",
  "protocolversion": 70015,
  "localservices": "000000000000000c",
  "localrelay": false,
  "timeoffset": 0,
  "networkactive": true,
  "connections": 0,
  "networks": [
    {
      "name": "ipv4",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "ipv6",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "onion",
      "limited": true,
      "reachable": false,
      "proxy": "",
      "proxy_randomize_credentials": false
    }
  ],
  "relayfee": 0.00001000,
  "incrementalfee": 0.00001000,
  "localaddresses": [
  ],
  "warnings": ""
}
```

### 지갑 설정 및 암호화

* 명령어: encryptwallet, walletpassphrase
  * 지갑에 foo 라는 암호를 걸어 사용
  * 지갑의 암호를 풀기 위해서는 walletpassphrase 사용하고 시간을 사용함, 단위는 초단위
    ```bash
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli encryptwallet foo
    wallet encrypted; Bitcoin server stopping, restart to run with encrypted wallet. The keypool has been flushed and a new HD seed was generated (if you are using HD). You need to make a new backup.

    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getinfo
    {
      "deprecation-warning": "WARNING: getinfo is deprecated and will be fully removed in 0.16. Projects should transition to using getblockchaininfo,  getnetworkinfo, and getwalletinfo before upgrading to 0.16",
      "version": 150000,
      "protocolversion": 70015,
      "walletversion": 139900,
      "balance": 0.00000000,
      "blocks": 0,
      "timeoffset": 0,
      "connections": 3,
      "proxy": "",
      "difficulty": 1,
      "testnet": false,
      "keypoololdest": 1531048626,
      "keypoolsize": 2000,
      "unlocked_until": 0,
      "paytxfee": 0.00000000,
      "relayfee": 0.00001000,
      "errors": ""
    }
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli walletpassphrase foo 360
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getinfo
    {
      "deprecation-warning": "WARNING: getinfo is deprecated and will be fully removed in 0.16. Projects should transition to using getblockchaininfo,  getnetworkinfo, and getwalletinfo before upgrading to 0.16",
      "version": 150000,
      "protocolversion": 70015,
      "walletversion": 139900,
      "balance": 0.00000000,
      "blocks": 0,
      "timeoffset": 558,
      "connections": 6,
      "proxy": "",
      "difficulty": 1,
      "testnet": false,
      "keypoololdest": 1531048626,
      "keypoolsize": 2000,
      "unlocked_until": 1531049048,
      "paytxfee": 0.00000000,
      "relayfee": 0.00001000,
      "errors": ""
    }
    ```

### 지갑 백업하기, 일반 텍스트 덤프하기, 복원하기

* 명령어: backupwallet, importwallet, dumpwallet
  * 지갑의 백업파일을 생성하고(backupwallet), 복원한다.(importwallet)
  * 또한 지갑 Human readerble 하게 만든다.(dumpwallet)
    ```
    anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli backupwallet
    error code: -1
    error message:
    backupwallet "destination"

    Safely copies current wallet file to destination, which can be a directory or a path with filename.

    Arguments:
    1. "destination"   (string) The destination directory or file

    Examples:
    > bitcoin-cli backupwallet "backup.dat"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "backupwallet", "params": ["backup.dat"] }' -H 'content-type: text/plain;' http://127.0.0.1:8332/

    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli backupwallet ./ryan-wallet
    (anaconda3-5.2.0) ryan@Wolf:~$ ls
    bitcoin-cli-help  bitcoin.conf  bitcoind-help.file  ryan-wallet  WorkHome
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli importwallet ./ryan-wallet
    error code: -4
    error message:
    Importing wallets is disabled in pruned mode
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli dumpwallet ./ryan-wallet
    error code: -13
    error message:
    Error: Please enter the wallet passphrase with walletpassphrase first.
    
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli walletpassphrase foo 360
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli dumpwallet ./ryan-wallet
    {
      "filename": "/home/ryan/./ryan-wallet"
    }
  
    (anaconda3-5.2.0) ryan@Wolf:~$ more ./ryan-wallet
    # Wallet dump created by Bitcoin v0.15.0
    # * Created on 2018-07-09T09:20:04Z
    # * Best block at time of backup was 367573 (0000000000000000093e25714ce75a7198cbecf01e879db80c858b516b124629),
    #   mined on 2015-07-30T01:36:08Z

    # extended private masterkey: xprv9s21ZrQH143K384BgrZ6XPxa4fXPtRCA9STAvyWny7D48MNL4qX2XHCoHN9kBVdGhAVG3K2xA8JPx43rqFrM
    fCTqrzehXSsVeoCAsXHMWi5

    KzzxMfHfPubzMfL1pmuexo9fzniTH2xjmBG85q6o3D8go559BfNm 2018-07-08T11:16:38Z change=1 # addr=11pLbXTSCXbfvCRdn9ZRybzqY3oo
    RvmwL hdkeypath=m/0'/0'/516'
    L4UqtdCq4z8DbAd4LXwcqDnxdiFBcrreBQdTsHBRctnEk4Bnjtre 2018-07-08T11:16:38Z change=1 # addr=11qVygvY4BRx7iHCAaDd2CijKGxq
    tpyqD hdkeypath=m/0'/0'/622'
    KyMKy7e5VueCiqnmhJoBvo5dHFPomVq62xbzAmgXh9bp96cQmzSG 2018-07-08T11:16:38Z change=1 # addr=1128TbLqjFKP5b3MCa4nC58SxQ8R
    VHAKVS hdkeypath=m/0'/0'/760'
    Kwdb1eB3zSCCRa6dMjR47NMFGeZB8Nsmpty4UjyrN9asZQUhMwbN 2018-07-08T11:16:38Z change=1 # addr=112hh8eMrHVQBwvULjnAXk8DEEQH
    Tm3ier hdkeypath=m/0'/0'/292'
    L1dSY85CBkXxcEFTH5Uw4MUdvuTk3CG5LRm1JLxXSsfAm78KtoEV 2018-07-08T11:16:38Z change=1 # addr=112oiRq9MAwQSXqJTqqz5agFCsFz
    Y2ayom hdkeypath=m/0'/0'/788'
    KzJqRCJBHaCPWbndVdk8t9NeQDf9cZvuVFqzz9ctanf7abETZ1e8 2018-07-08T11:16:38Z change=1 # addr=112ueJWe4Y1Jj4GxKSJqAKb4bw8E
    S7mpmX hdkeypath=m/0'/0'/120'
    KxvDX4dJzrcByudQNmTiqWgLa6v2tFVUoSgQNrkxRxD7QarZgQVG 2018-07-08T11:16:38Z change=1 # addr=112yo6t7vxJLvJCDJz8zuUijdhav
    fwxdPE hdkeypath=m/0'/0'/541'
    KzCT5NTmnzyPbUSN7KwUfvNqqdTHbhVp431GrLbndeQL3DNhqcKa 2018-07-08T11:16:38Z change=1 # addr=1136mMVZ2MaEv7Un3Nq8aYEttqUZ
    trbNH8 hdkeypath=m/0'/0'/478'
    ```

### 지갑 주소 생성하기와 거래 수신하기

* 명령어: getnewaddress, getreceivedbyaddress, listtransactions, getaddressesbyaccount, getbalance
    ```
    // 주소를 생성하고
    // 외부에서 이 주소로 송금해 볼 것
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getnewaddress
    1Nrko8wi8cDMpJtYa8GF2WGpWao37AnQP7

    // 해당 주소로 돈이 들어왔는지 확인
    // 0은 승인회수로 0을 생략하면 bitcoin.conf 에 minconf 설정된 최소한 승인을 받은 금액만 확인 될 것임
    // minconf란 한 거래가 잔액에 반영되기까지 필요한 최소 승인 설정 값
    // 바로 승인이 되지 않으면 잔액 0으로 표시 될 것임
    $ bitcoind-cli getreceivedbyaddress 1Nrko8wi8cDMpJtYa8GF2WGpWao37AnQP7 0

    // 해당 지갑이 수신한 모든 거래내역을 볼 수 있음
    $ bitcoind-cli listtransactions

    // 지갑이 보유하고 있는 모든 주소 목록
    $ bitcoind-cli getaddressesbyaccount ""

    $ bitcoind-cli getbalance
    ```

### 거래내역 살펴보기 및 디코딩하기(Exploring and Decoding Transactions)

* Commands: getrawtransaction , decoderawtransaction
  * 위 커맨드로 Tx에 대한 정보를 확인할 수 있음
  * 예를 들어 Alice가 Bob 의 카페에서 커피를 사고 커피값을 비트코인으로 지불 했던 거래의 txid:
    * 0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2
  * getrawtransaction
    ```
    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getrawtransaction 0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2

    0100000001186f9f998a5aa6f048e51dd8419a14d8a0f1a8a2836dd734d2804fe65fa35779000↵
    000008b483045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4↵
    ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e3813014↵
    10484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc54123363767↵
    89d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adfffffffff0260e3160000000↵
    0001976a914ab68025513c3dbd2f7b92a94e0581f5d50f654e788acd0ef8000000000001976a9↵
    147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac00000000

    (anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli decoderawtransaction 0100000001186f9f998a5aa6f048e51dd8419a14d8↵
    a0f1a8a2836dd734d2804fe65fa35779000000008b483045022100884d142d86652a3f47ba474↵
    6ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298↵
    cad530a863ea8f53982c09db8f6e381301410484ecc0d46f1918b30928fa0e4ed99f16a0fb4fd↵
    e0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa↵
    336a8d752adfffffffff0260e31600000000001976a914ab68025513c3dbd2f7b92a94e0581f5↵
    d50f654e788acd0ef8000000000001976a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a8↵
    88ac00000000

    {
      "txid": "0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2",
      "size": 258,
      "version": 1,
      "locktime": 0,
      "vin": [
        {
          "txid": "7957a35fe64f80d234d76d83a2...8149a41d81de548f0a65a8a999f6f18",
          "vout": 0,
          "scriptSig": {
            "asm":"3045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1decc...",
            "hex":"483045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1de..."
          },
          "sequence": 4294967295
        }
      ],
      "vout": [
        {
          "value": 0.01500000,
          "n": 0,
          "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 ab68...5f654e7 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a914ab68025513c3dbd2f7b92a94e0581f5d50f654e788ac",
            "reqSigs": 1,
            "type": "pubkeyhash",
            "addresses": [
              "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
            ]
          }
        },
        {
          "value": 0.08450000,
          "n": 1,
          "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 7f9b1a...025a8 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac",
            "reqSigs": 1,
            "type": "pubkeyhash",
            "addresses": [
              "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK"
            ]
          }
        }
      ]
    }
    ```
    * [Blockcahin Info 확인](https://www.blockchain.com/en/btc/tx/0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2)

* Tip
  * Txid 는 TX가 끝날 때까지 완전히 유효(authoritative) 하지 않음
  * 또한, Tx hash가 표시되지 않았다고 거래가 진행되지 않은 것은 아님
    * '거래 가변성(Transaction Malleability)'이라는 블럭체인의 속성임
    * 거래 가변성이란 Tx의 Hash는 거래가 완료되고 블럭에 포함되기전에 변경될 수 있음을 의미
    * 거래가 완료(Confirmed)되고 나서야 txid 는 비가역적, 유효(immutable, authoritative)지는 것임
    * [비트코인 버그, Transaction Malleability?](http://blog.naver.com/PostView.nhn?blogId=allthatbtc&logNo=20205397111))

### Exploring Blocks

```
(anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getblockcount
368983
(anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getblockhash 368983
0000000000000000047b1540e87ce7ea1a736fa3d22734315f91b3eb91e919dd
(anaconda3-5.2.0) ryan@Wolf:~$ bitcoin-cli getblock 0000000000000000047b1540e87ce7ea1a736fa3d22734315f91b3eb91e919dd
{
  "hash": "0000000000000000047b1540e87ce7ea1a736fa3d22734315f91b3eb91e919dd",
  "confirmations": 20,
  "strippedsize": 227949,
  "size": 227949,
  "weight": 911796,
  "height": 368983,
  "version": 3,
  "versionHex": "00000003",
  "merkleroot": "bf0a2803579b8bfd2ae879e98edf0d4aa68fa7e674ff49f8c3797eb6e6a3fbf2",
  "tx": [
    "e92dc5b4ab03b0da6fbae9a3f4d11bc4d5a6c98e140c9fac60de2af7b7491230",
    "214fd9c980023fb7baf03d759008f18769858e005026978ab53d0e92b6f7a985",
    "2c55de2ce1dfebbfe4803973f751f43f2bebcc4276ca4f7b0cbc8cc3dd1d98ec",
    "71f3f874746647af1e5c1cc4474a487969e51f85612afca248bc5567fe1c319b",
    "c26eeb80ff36cff2b7b68f768aaf8f2121ca23b355ea1b51b7942011e02221ae",
    "865b73625671eb90558a8697bc798570672952dbfd9c68d42dd29ab201c48331",
    "c1fb06d8e8cc7de5e8ef01d7219647129603566beb97add71f84cc56bc1d8a6f",
    "48063c75a7d2e332ba995c1747e7cd922d7f8d612c66ab8ecfd18209eff0a129",
    "d292edb3d0bab2a67711e728a4b7f7d9640c526467ad798ac0f4d48a0910b3e6",
    "a7e6f89df48e2a876e0efa7bc139476767b20f21e96407073f1a5ec7ea93a821",
    "79f0ee11a3582abc6c96c9fde4e052bfb4df223fd9c666cf3b33eed37371b7b1",
    "212c76734fe820f5aa625d293fd39c2b45ae45a89594a80591361037476f92da",
    "7614928335b0e5b4915f4ac1fa269e6f3a844d7af751945972e8c2b9219edc99",
    "d77783706a7b1ae7422618c44bbf2d34c0a78a55e77112e7a939a6e1d3005005",
    "2bbbd4a796e014328a4137a1eea7db1460df34003f68629bebbaf13dc18b8993",
    "69f7dce654a53ac94878ca91e02275dc3ba5a23706068a188fca5ee00a44e701",
    "9d7e37b57d72f3fce36b263affdfdf03c8f40803c919d310ef38437ee0880a5e",
    "d49655e7266e8f5cac2d1d0d323ae2f34cd51b57839993d2c992b5379fce31b6",
    "0049feb8d391782d34aa4033dc1a59164a387e8db13138be5a2cd137adfdf431",
    "d5da2a56fd9dff7301305c8c32d57e7411ee882c36bd4d1906f67315fbbf3ea9",
    "b4aba29e51423b070b31b5a41c6fce1caf8b4b4698b884ae43ddc0e0d278289f",
    "92551e120ac23bd3d408d9c464c0a795161a72e1a77b7f44c1f5578c88553d26",
    "664862d47b94a913f8d61676f23966cbe6b6d4ec94990575a5df3c630ed7ec68",
    "64153cc0e2c3d389705aaf7e7e2664a2325fd3a9b55fd8b39a205924b631c204",
    "b41cfbdb9a957e9d9161ba65a3071485a0b8fdf49d097fb3f5634d70477fa78c",
    "98c31a9d9841f6b4540b55e540a719f1c5879c153a3d20949a1122cfec215900",
    "2f6ed8307771c45b85d719183a3d6250b85572921fddc5dc9de44fa9b3ed923d",
    "dc21502527ca534b3dd724a9d24c442c5760fae9906c430261ba070cc4ebcf94",
    - 생략 -
    "ce153d8f2e59bc0b0acb23555d142d5f566d19ba753ecf11245e3d12ee7bfa91",
    "f58f39d64955419d82081455f08487568eb04c72edeb64612d53aa97f76400ae",
    "32eb6e01fabb3c042669640f0cb4a82bfe8de49588ee926dfed0b27795d30002",
    "189989fabb3d82e910240b1efaf725e1013dee3676c7c018d13d7c0b01550ad1",
    "004a3a80e621b6d5fd266b03e9fcb04757f782159ccc338a94ab103e4921758b",
    "f03d6b846b8caa95fbbb7a50339d4f32183dd712fd6a866bc2dc4fcf36a6ea26",
    "26baeb45dd7240c56ddb4096b096b51ba85aad610fa91970a163eaafcfa2ed1a",
    "82fa0667e8669fc75e23d747a7ad99f1b927b52a28a591aceac2cd30071cb634",
    "df1a8bd87ad18af87e5dd2e1990b871483332c0e29e458c6064a85c7e498b625",
    "ba8525171730433a06fd6f902d60d3dd47cad0b2292bd76204dc8aa3223c9f7f",
    "90bb6b9bb19efb0f4ae9dfe94d6c19065410db39288165c4b28fe6db9f24e65d",
    "090f6f5107c34ba7195fff93d2746e7d70abc4f96eb96c95e5ae1947175c9612"
  ],
  "time": 1439058413,
  "mediantime": 1439055228,
  "nonce": 2859968939,
  "bits": "1814dd04",
  "difficulty": 52699842409.34701,
  "chainwork": "00000000000000000000000000000000000000000009193789c89ff3fcb67b40",
  "previousblockhash": "00000000000000001257f5d580dae939209502a704a25e9c9f74ea36ae744a68",
  "nextblockhash": "00000000000000000b74719ffba4a00d90ccd352928318ddc080447fd374d11e"
}
```

### Using Bitcoin Core's Programmatic Interface

```
curl --user ryan --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8332/
```

* python interface 사용
  * python-bitcoinlib 설치 필요
    ```bash
    (anaconda3-5.2.0) ryan@Wolf:~$ sudo apt-get install libssl-dev
    [sudo] password for ryan:
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    libssl-dev is already the newest version (1.0.2g-1ubuntu4.13).
    0 upgraded, 0 newly installed, 0 to remove and 14 not upgraded.
    ```


    (anaconda3-5.2.0) ryan@Wolf:~$ pip install python-bitcoinlib
    Collecting python-bitcoinlib
      Downloading https://files.pythonhosted.org/packages/30/03/fb7df95fe89baede202cf3fe65e65bea4bf863061b5e8f59b12dab538240/python_bitcoinlib-0.10.1-py2.py3-none-any.whl (88kB)
        100% |████████████████████████████████| 92kB 595kB/s
    distributed 1.21.8 requires msgpack, which is not installed.
    Installing collected packages: python-bitcoinlib
    Successfully installed python-bitcoinlib-0.10.1
    (anaconda3-5.2.0) ryan@Wolf:~$ pip install msgpack
    Collecting msgpack
      Downloading https://files.pythonhosted.org/packages/22/4e/dcf124fd97e5f5611123d6ad9f40ffd6eb979d1efdc1049e28a795672fcd/msgpack-0.5.6-cp36-cp36m-manylinux1_x86_64.whl (315kB)
        100% |████████████████████████████████| 317kB 3.1MB/s
    Installing collected packages: msgpack
    Successfully installed msgpack-0.5.6
    
    ```

  * Block 수 확인
    ```python
    #!/usr/bin/env python3
    # -*- coding:utf8 -*-
    
    #
    # filename: rpc_example.py
    #
    from bitcoin.rpc import RawProxy
    ```


    # Create a connection to local Bitcoin Core node
    p = RawProxy()
    
    # Run the getblockchaininfo command, store the resulting data in info
    info = p.getblockchaininfo()
    
    # Retrieve the 'blocks' element from the info
    print(info['blocks'])
    ```
    ```bash
    (anaconda3-5.2.0) ryan@Wolf:~$ python rpc_example.py
    372620
    ```

  * 구매트랜잭션 ID로 트랜잭션 확인
  ```python
  #!/usr/bin/env python3
  # -*- coding:utf8 -*-

  #
  # filename: rpc_transaction.py
  #
  from bitcoin.rpc import RawProxy

  p = RawProxy()

  # Alice's transaction ID
  txid = "0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2"

  # First, retrieve the raw transaction in hex
  raw_tx = p.getrawtransaction(txid)

  # Decode the transaction hex into a JSON object
  decoded_tx = p.decoderawtransaction(raw_tx)

  # Retrieve each of the outputs from the transaction
  for output in decoded_tx['vout']:
      print(output['scriptPubKey']['addresses'], output['value'])

  ```
  ```bash
  (anaconda3-5.2.0) ryan@Wolf:~$ python rpc_transaction.py
  ['16kktFTqsruEfPPphW4YgjktRF28iT8Dby'] 50.00000000
  ```

* 거래가 진행되는 모든 Output 보기
  ```python
  #!/usr/bin/env python3
  # -*- coding:utf8 -*-

  #
  # filename: rpc_block.py
  #
  from bitcoin.rpc import RawProxy

  p = RawProxy()

  # The block height where Alice's transaction was recorded
  blockheight = 277316

  # Get the block hash of block with height 277316
  blockhash = p.getblockhash(blockheight)

  # Retrieve the block by its hash
  block = p.getblock(blockhash)

  # Element tx contains the list of all transaction IDs in the block
  transactions = block['tx']

  block_value = 0

  # Iterate through each transaction ID in the block
  for txid in transactions:
      tx_value = 0
      # Retrieve the raw transaction by ID
      raw_tx = p.getrawtransaction(txid)
      # Decode the transaction
      decoded_tx = p.decoderawtransaction(raw_tx)
      # Iterate through each output in the transaction
      for output in decoded_tx['vout']:
          # Add up the value of each output
          tx_value = tx_value + output['value']

      # Add the value of this transaction to the total
      block_value = block_value + tx_value

  print("Total value in block: ", block_value)

  ```