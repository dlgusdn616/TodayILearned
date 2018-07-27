## Running a Bitcoin Core Node

### Configuring the Bitcoin Core Node

*  비트코인 코어는 실행할 때마다 Configuration 파일을 찾는다.

* 비트코인 Configuration 위치는 `bitcoind -printtoconsole` 명령어로 데몬의 실행내역을 확인해보면 알 수 있다.

  * 그러나 최하단의 출력 결과를 따라가면 **bitcoin.conf** 파일은 존재하지 않으므로 따로 만들도록 한다.

    ```shell
    waca@waca-VirtualBox:~$ bitcoind -printtoconsole
    2018-07-27T02:23:44Z Bitcoin Core version v0.16.99.0-1211b15 (release build)
    2018-07-27T02:23:44Z InitParameterInteraction: parameter interaction: -whitelistforcerelay=1 -> setting -whitelistrelay=1
    2018-07-27T02:23:44Z Assuming ancestors of block 0000000000000000005214481d2d96f898e3d5416e43359c145944a909d242e0 have valid signatures.
    2018-07-27T02:23:44Z Setting nMinimumChainWork=000000000000000000000000000000000000000000f91c579d57cad4bc5278cc
    2018-07-27T02:23:44Z Using the 'sse4(1way),sse41(4way),avx2(8way)' SHA256 implementation
    2018-07-27T02:23:44Z Using RdRand as an additional entropy source
    ^N2018-07-27T02:23:46Z Default data directory /home/waca/.bitcoin
    2018-07-27T02:23:46Z Using data directory /home/waca/.bitcoin
    2018-07-27T02:23:46Z Using config file /home/waca/.bitcoin/bitcoin.conf
    ```

  * 출력 결과 중 알아둬야 할 주요 정보들을 아래에 별도로 기술해 놓았다. 참조하도록 한다.

    ```shell
    Default data directory /home/user/.bitcoin
    Using data directory /home/user/.bitcoin
    Using config file /home/user/.bitcoin/bitcoin.conf
    Generated RPC authentication cookie /home/user/.bitcoin/.cookie
    Using wallet directory /home/user/.bitcoin/wallets
    Using BerkelyDB version Berkely DB 4.8.30: (April 9, 2010)
    Using wallet wallet.dat
    BerkeleyEnvironment::Open: LogDir=/home/user/.bitcoin/wallets/database ErrorFile=/home/user/.bitcoin/wallets/db.log
    Cache configuration:
    * Using 2.0MiB for block index database
    * Using 8.0MiB for chain state database
    * Using 440.0MiB for in-memory UTXO set (plus up to 286.1MiB of unused mempool space)
    Opening LevelDB in /home/waca/.bitcoin/blocks/index
    Using obfuscation key for /home/waca/.bitcoin/blocks/index: 0000000000000000
    Opening LevelDB in /home/waca/.bitcoin/chainstate
    Using obfuscation key for /home/waca/.bitcoin/chainstate: 6bb80eeb9c2f701f
    Loaded best chain: hashBestChain=0000000000000000001ad5c755378e88f24090da83382d18f9a062f3fdeedadd height=533730 date=2018-07-26T05:30:39Z progress=0.999204
    ```

  * Ctrl + C로 데몬을 종료시킨 후에 아래의 과정을 진행해보도록 한다.

  * 전체 실행 결과가 굉장히 길지만, 반드시 살펴봐야 하기에 아래에 쭈욱 첨부해두었다. 내용이 길지만 차분하게 읽고 이해하도록 하자.

  ```shell
  waca@waca-VirtualBox:~$ bitcoind -printtoconsole
  2018-07-27T02:23:44Z Bitcoin Core version v0.16.99.0-1211b15 (release build)
  2018-07-27T02:23:44Z InitParameterInteraction: parameter interaction: -whitelistforcerelay=1 -> setting -whitelistrelay=1
  2018-07-27T02:23:44Z Assuming ancestors of block 0000000000000000005214481d2d96f898e3d5416e43359c145944a909d242e0 have valid signatures.
  2018-07-27T02:23:44Z Setting nMinimumChainWork=000000000000000000000000000000000000000000f91c579d57cad4bc5278cc
  2018-07-27T02:23:44Z Using the 'sse4(1way),sse41(4way),avx2(8way)' SHA256 implementation
  2018-07-27T02:23:44Z Using RdRand as an additional entropy source
  ^N2018-07-27T02:23:46Z Default data directory /home/waca/.bitcoin
  2018-07-27T02:23:46Z Using data directory /home/waca/.bitcoin
  2018-07-27T02:23:46Z Using config file /home/waca/.bitcoin/bitcoin.conf
  2018-07-27T02:23:46Z Using at most 125 automatic connections (1024 file descriptors available)
  2018-07-27T02:23:46Z Using 16 MiB out of 32/2 requested for signature cache, able to store 524288 elements
  2018-07-27T02:23:46Z Using 16 MiB out of 32/2 requested for script execution cache, able to store 524288 elements
  2018-07-27T02:23:46Z Using 0 threads for script verification
  2018-07-27T02:23:46Z scheduler thread start
  2018-07-27T02:23:46Z HTTP: creating work queue of depth 16
  2018-07-27T02:23:46Z No rpcpassword set - using random cookie authentication.
  2018-07-27T02:23:46Z Generated RPC authentication cookie /home/waca/.bitcoin/.cookie
  2018-07-27T02:23:46Z HTTP: starting 4 worker threads
  2018-07-27T02:23:46Z Using wallet directory /home/waca/.bitcoin/wallets
  2018-07-27T02:23:46Z init message: Verifying wallet(s)...
  2018-07-27T02:23:46Z Using BerkeleyDB version Berkeley DB 4.8.30: (April  9, 2010)
  2018-07-27T02:23:46Z Using wallet wallet.dat
  2018-07-27T02:23:46Z BerkeleyEnvironment::Open: LogDir=/home/waca/.bitcoin/wallets/database ErrorFile=/home/waca/.bitcoin/wallets/db.log
  2018-07-27T02:23:46Z Cache configuration:
  2018-07-27T02:23:46Z * Using 2.0MiB for block index database
  2018-07-27T02:23:46Z * Using 8.0MiB for chain state database
  2018-07-27T02:23:46Z * Using 440.0MiB for in-memory UTXO set (plus up to 286.1MiB of unused mempool space)
  2018-07-27T02:23:46Z init message: Loading block index...
  2018-07-27T02:23:47Z Opening LevelDB in /home/waca/.bitcoin/blocks/index
  2018-07-27T02:23:47Z Opened LevelDB successfully
  2018-07-27T02:23:47Z Using obfuscation key for /home/waca/.bitcoin/blocks/index: 0000000000000000
  2018-07-27T02:23:53Z LoadBlockIndexDB: last block file = 1325
  2018-07-27T02:23:53Z LoadBlockIndexDB: last block file info: CBlockFileInfo(blocks=149, size=120945075, heights=533534...533730, time=2018-07-25...2018-07-26)
  2018-07-27T02:23:53Z Checking all blk files are present...
  2018-07-27T02:23:54Z Opening LevelDB in /home/waca/.bitcoin/chainstate
  2018-07-27T02:23:59Z Opened LevelDB successfully
  2018-07-27T02:24:00Z Using obfuscation key for /home/waca/.bitcoin/chainstate: 6bb80eeb9c2f701f
  2018-07-27T02:24:00Z Loaded best chain: hashBestChain=0000000000000000001ad5c755378e88f24090da83382d18f9a062f3fdeedadd height=533730 date=2018-07-26T05:30:39Z progress=0.999204
  2018-07-27T02:24:00Z init message: Rewinding blocks...
  2018-07-27T02:24:03Z init message: Verifying blocks...
  2018-07-27T02:24:03Z Verifying last 6 blocks at level 3
  2018-07-27T02:24:03Z [0%]...[16%]...[33%]...[50%]...[66%]...[83%]...[99%]...[DONE].
  2018-07-27T02:25:06Z No coin database inconsistencies in last 6 blocks (8909 transactions)
  2018-07-27T02:25:06Z  block index           79608ms
  2018-07-27T02:25:06Z init message: Loading wallet...
  2018-07-27T02:25:06Z nFileVersion = 169900
  2018-07-27T02:25:06Z Keys: 2001 plaintext, 0 encrypted, 2001 w/ metadata, 2001 total. Unknown wallet records: 1
  2018-07-27T02:25:06Z  wallet                  144ms
  2018-07-27T02:25:06Z setKeyPool.size() = 2000
  2018-07-27T02:25:06Z mapWallet.size() = 0
  2018-07-27T02:25:06Z mapAddressBook.size() = 0
  2018-07-27T02:25:06Z mapBlockIndex.size() = 533731
  2018-07-27T02:25:06Z nBestHeight = 533730
  2018-07-27T02:25:06Z Bound to [::]:8333
  2018-07-27T02:25:06Z Bound to 0.0.0.0:8333
  2018-07-27T02:25:06Z init message: Loading P2P addresses...
  2018-07-27T02:25:06Z torcontrol thread start
  2018-07-27T02:25:06Z Leaving InitialBlockDownload (latching to false)
  2018-07-27T02:25:07Z Loaded 23118 addresses from peers.dat  307ms
  2018-07-27T02:25:07Z init message: Loading banlist...
  2018-07-27T02:25:07Z init message: Starting network threads...
  2018-07-27T02:25:07Z init message: Done loading
  2018-07-27T02:25:07Z msghand thread start
  2018-07-27T02:25:07Z opencon thread start
  2018-07-27T02:25:07Z addcon thread start
  2018-07-27T02:25:07Z dnsseed thread start
  2018-07-27T02:25:07Z net thread start
  2018-07-27T02:25:15Z New outbound peer connected: version: 70015, blocks=533862, peer=0
  2018-07-27T02:25:18Z Loading addresses from DNS seeds (could take a while)
  2018-07-27T02:25:31Z 252 addresses found from DNS seeds
  2018-07-27T02:25:31Z dnsseed thread exit
  2018-07-27T02:25:33Z UpdateTip: new best=000000000000000000083a7cdb805fd5ca7b07328f92d1bf3823b945ae90991a height=533731 version=0x20000000 log2_work=89.316666 tx=330560347 date='2018-07-26T06:11:18Z' progress=0.999229 cache=1.7MiB(12616txo)
  2018-07-27T02:25:34Z Imported mempool transactions from disk: 1725 succeeded, 120 failed, 0 expired, 0 already there
  2018-07-27T02:25:49Z Pre-allocating up to position 0x1000000 in rev01325.dat
  2018-07-27T02:25:49Z UpdateTip: new best=00000000000000000018b4e3fdae6cf3a82c89588f5837a31f4f2d55fed36f37 height=533732 version=0x20ffff00 log2_work=89.316708 tx=330562635 date='2018-07-26T06:14:04Z' progress=0.999231 cache=2.9MiB(21683txo) warning='1 of last 100 blocks have unexpected version'
  2018-07-27T02:25:55Z UpdateTip: new best=000000000000000000097f59f6ceb326f36d23064fda1af80a522ff468033fac height=533733 version=0x20000000 log2_work=89.316749 tx=330563163 date='2018-07-26T06:16:09Z' progress=0.999232 cache=3.5MiB(26896txo) warning='1 of last 100 blocks have unexpected version'
  2018-07-27T02:25:55Z New outbound peer connected: version: 70015, blocks=533862, peer=1
  2018-07-27T02:25:55Z New outbound peer connected: version: 70015, blocks=533862, peer=2
  2018-07-27T02:26:02Z UpdateTip: new best=0000000000000000002cdd43061b8e16dee83935b07b323e543b39bbbe414de0 height=533734 version=0x20000000 log2_work=89.316791 tx=330564091 date='2018-07-26T06:22:50Z' progress=0.999236 cache=3.9MiB(29951txo) warning='1 of last 100 blocks have unexpected version'
  2018-07-27T02:26:02Z New outbound peer connected: version: 70015, blocks=533862, peer=4
  2018-07-27T02:26:21Z UpdateTip: new best=0000000000000000001e87980e46b67b56c1d93f13591cd2ab6250613ac94361 height=533735 version=0x20000000 log2_work=89.316833 tx=330566356 date='2018-07-26T06:40:56Z' progress=0.999248 cache=5.1MiB(38066txo) warning='1 of last 100 blocks have unexpected version'
  2018-07-27T02:26:29Z UpdateTip: new best=00000000000000000035565ab93e8332c1b6c8fccb7a38eaab765b5c873f8f25 height=533736 version=0x20000000 log2_work=89.316874 tx=330568170 date='2018-07-26T06:52:59Z' progress=0.999255 cache=5.8MiB(43697txo) warning='1 of last 100 blocks have unexpected version'
  2018-07-27T02:26:29Z UpdateTip: new best=0000000000000000000dbf699d783bf38179163f41725ba57b36a21bd37b9d48 height=533737 version=0x20000000 log2_work=89.316916 tx=330568221 date='2018-07-26T06:53:26Z' progress=0.999255 cache=5.8MiB(43863txo) warning='1 of last 100 blocks have unexpected version'
  2018-07-27T02:26:29Z New outbound peer connected: version: 70015, blocks=533862, peer=5
  2018-07-27T02:26:37Z UpdateTip: new best=000000000000000000117347d706c1307e2e9aef1f61644ba01a48c99da51ce3 height=533738 version=0x3fffe000 log2_work=89.316957 tx=330569980 date='2018-07-26T07:05:40Z' progress=0.999263 cache=6.5MiB(49370txo) warning='2 of last 100 blocks have unexpected version'
  2018-07-27T02:26:42Z UpdateTip: new best=00000000000000000028b8525f410587a0d30e5370495ed8b064909fd7c1a779 height=533739 version=0x20000000 log2_work=89.316999 tx=330571165 date='2018-07-26T07:12:36Z' progress=0.999267 cache=7.0MiB(53233txo) warning='2 of last 100 blocks have unexpected version'
  2018-07-27T02:26:42Z New outbound peer connected: version: 70015, blocks=533862, peer=6
  ```

  

* 비트코인 코어는 100개가 넘는 옵션을 제공한다.

  * 아래의 명령어를 통해 적용할 수 있는 옵션이 어떤 것들이 있는지 살펴보도록 한다.

  `waca@waca-Virtualbox:~$ bitcoind --help`

* 주요 옵션 + 개인 의견

  * -alertnotify=<cmd>
    지정된 명령 또는 스크립트를 실행하여 노드의 소유자(일반적으로 전자 메일)에 경고를 전송. 관련된 경고가 도착하거나 상당한 길이의 포크를 발견했을 때 발생한다.
    `alertnotify=echo %s | mail -s "Bitcoin Alert" admin@gmail.com`  와 같이 사용한다. %s는 실행시 경고 메시지로 대체될 것이다.
  * -conf=<file>
    config 파일 위치를 변경해서 사용할 수 있다. 위치의 경우 디폴트로 앞에 datadir의 위치가 담기게 된다. 따라서 디폴트는 `bitcoin.conf` 가 되는 것이고 앞에는 `/home/user/.bitcoin` 이 디폴트로 삽입된다고 생각하면 된다.
  * -blocksdir=<dir>
    블록들이 저장될 디렉토리를 선택한다. (디폴트: <datadir>/blocks) 첫 실행시 `Default data directory /home/user/.bitcoin` 와 같은 결과가 나왔으므로 <datadir>는 `/home/user/.bitcoin`로 대체될 것이다.
  * -datadir=<dir>
    데이터가 저장될 디렉토리를 선택한다.
  * -daemon
    백그라운드로 데몬을 실행한다.
  * -dbcache=<n>
    데이터베이스 캐시 사이즈를 메가바이트 단위로 설정한다. (4 to 16384, default: 450)