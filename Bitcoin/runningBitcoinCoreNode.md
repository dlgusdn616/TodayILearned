## Running a Bitcoin Core Node

### Configuring the Bitcoin Core Node

* 비트코인 코어는 실행할 때마다 Configuration 파일을 찾는다.

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

  * -datadir=<dir>
    데이터가 저장될 디렉토리를 선택한다.

  * -blocksdir=<dir>
    블록들이 저장될 디렉토리를 선택한다. (디폴트: <datadir>/blocks) 첫 실행시 `Default data directory /home/user/.bitcoin` 와 같은 결과가 나왔으므로 <datadir>는 `/home/user/.bitcoin`로 대체될 것이다.

  * -daemon
    백그라운드로 데몬을 실행한다.

  * -dbcache=<n>
    데이터베이스 캐시 사이즈를 메가바이트 단위로 설정한다. (4 to 16384, default: 450)

  * -maxmempool=<n>
    트랜잭션 메모리 풀 사이즈를 <n>메가바이트 미만으로 잡는다. (디폴트는300).

  * -maxorphantx=<n>
    최대 <n>개의 unconnectable 트랜잭션들을 메모리에 유지한다. (디폴트는 100).

  * -maxpoolexpiry=<n>
    메모리 풀에 <n> 시간 보다 길게 보관되어 있는 트랜잭션들을 보관하지 않는다. (디폴트는 336).

  * -par=<n>
    스크립트 검증 스레드 개수를 지정한다. (-1 부터 16까지, 0 = auto, <0 = leave that many cores free, default: 0)

  * -persistmempool
    셧다운 시에 멤풀을 저장하고 재시작 시 로드할지를 정할 것인지의 여부 (디폴트: 1)

  * -prune=<n>
    이전 블록을 삭제하여 디스크 공간을 절약할 수 있다. 특정 블록을 삭제하기 위해 pruneblockchain RPC를 호출할 수도 있고, target size를 MiB로 정하여 자동으로 오래된 블록을 pruning하는 방식을 택할 수도 있다. 이 모드는 -txindex 옵션과 -rescan옵션에는 호환되지 않는다. 

    > 이 옵션을 사용하다 보면 전체 블록체인을 다시 다운로드 받아야 하는 상황이 생길 수 있으니 충분히 주의하도록 한다.

    디폴트는 0: disable pruning blocks, 1 = allow manual pruning via RPC, >550 = automatically prune block files to stay under the specified target size in MiB

  * -reindex
    체인 상태와 디스크 내의 blk*.dat 파일에 저장되어 있는 블록 인덱스를 다시 빌드한다.

  * -reindex-chainstate
    현재 인덱싱된 블록들로부터 체인 상태를 다시 빌드한다.

  * -txindex
    모든 트랜잭션의 인덱스를 유지한다. 이 옵션을 사용하면  getrawtransaction rpc로 모든 tx에 접근이 가능하다. Full-index node라고 한다.



* Connection options:
  * -addnode=<ip>
    -특정 노드에 연결하고 가능한 연결을 유지하려 한다.
  * -bind<addr>
    주어진 주소를 묶고 항상 해당 주소들에 대해서는 요청대기 상태로 기다린다. IPv6를 위해 [host]:port 노테이션을 사용한다.
  * -connect=<ip>
    오직 특정 노드(들)에게만 연결을 시도한다; -noconnect 또는 -connect=0 이 따로 사용된다면 자동 연결을 차단한다.
  * -discover
    Discover own IP addresses (디폴트: 1 when listening and no -externalip or -proxy)
  * -dns
    -addnode, -seednode and -connect를 위해 DNS 룩업을 허용한다. 
  * -dnsseed
    피어들의 주소를 DNS 룩업에서 쿼리(if low on addresses) 한다. (디폴트: 1 unless -connect/-noconnect)
  * -externalip=<ip>
    나만의 퍼블릭 주소를 지정
  * -listen
    바깥에서의 연결을 받아들인다. (디폴트: 1 if no -proxy or -connect/-noconnect)
  * -listenonion 
    자동으로 Tor 히든 서비스를 생성한다 (디폴트: 1)
  * -maxconnections=<n>
    최대 <n>개의 피어와의 연결을 유지한다. (디폴트: 125)
  * -maxreceivebuffer=<n>
    커넥션별 최대 수신 버퍼 크기, <n> * 1000 bytes (default: 5000)
  * -maxsendbuffer=<n>
    커넥션별 최대 전송 버퍼 크기, <n> * 1000 bytes (default: 1000)
  * -maxtimeadjustment
    Maximum allowed median peer time offset adjustment. Local perspective of time may be influenced by peers forward or backward by this amount. (default: 4200 seconds)
  * -onion=[ip:port]
    Tor 히든 서비스들 너머의 피어들에게 도달하기 위해 분리된 SOCKS5 프록시를 사용한다. (디폴트: -proxy)
  * -onlynet=<net>
    네트워크 <net> 내의 노드들에만 연결한다. (ipv4, ipv6 or onion)
  * -permitbaremultisig
    Relay non-P2SH multisig (디폴트: 1)
  * -peerbloomfilters
    블록들과 트랜잭션들을 필터링하는 블룸 필터를 지원한다. (디폴트: 1)
  * -port=<port>
    <port>에서 연결을 위한 수신 대기를 한다. (디폴트: 8333 or testnet: 18333)
  * -proxy=[ip:port]
    SOCKS5 프록시를 통해 연결한다.
  * -proxyrandomize
    Randomize credentials for every proxy connection. This enables Tor stream isolation (디폴트: 1)
* Wallet options:
  * -disablewallet
    지갑을 로드하지 않고 wallet RPC 콜 또한 허락하지 않는다.
  * -keypool=<n>
    키 풀 사이즈를 <n>으로 설정 (디폴트: 100)
  * -fallbackfee=<amt>
    A fee rate (in BTC/kB) that will be used when fee estimation has insufficient data (디폴트: 0.0002)
  * -mintxfee=<amt>
    Fees (in BTC/kB) smaller than this are considered zero fee for transaction creation (디폴트: 0.0001)
  * -paytxfee=<amt>
    Fee (in BTC/kB) to add to transactions you send (디폴트: 0.00)
  * -rescan 
    프로그램 시작 시, 누락된 지갑 트랜잭션이 블록체인 내에 존재하는지 다시 스캔
  * salvagewallet 
    Attempt to recover private keys from a corrupt wallet on startup
  * -txconfirmtarget=<n>
    If paytxfee is not set, include enough fee so transactions begin confirmation on average within n blocks (디폴트: 6)
  * -wallet=<file>
    Specify wallet file (within data directory) (디폴트: wallet.dat)
  * -walletbroadcast
    Make the wallet broadcast transactions (디폴트: 1)
* ZeroMQ notification options:
* Debugging/Testing options:
  * -uacomment=<cmt>
    Append comment to the user agent string
  * -printtoconsole
    Send trace/debug info to console instead of debug.log file
* Chain Selection options:
  * -testnet
* Node relay options:
  * -bytespersigop
    Equivalent bytes per sigop in transactions for relay and mining (디폴트: 20)
  * -datacarrier
    Relay and mine data carrier transactions (디폴트: 1)
  * -datacarriersize
    Maximum size of data in data carrier transactions we relay and mine (디폴트: 83)
  * -mempoolreplacement
    Enable transaction replacement in the memory pool (디폴트: 1)
* Block creation options:
  * -blockmaxweight=<n>
    Set maximum BIP141 block weight (디폴트: 3000000)
  * -blockmaxsize=<n>
    Set maximum block size in bytes (디폴트: 750000)
* RPC server options:
  * -server
    Accept command line and JSON-RPC commands
  * -rest
    Accept public REST requests (디폴트: 0)
  * -rpccookiefile=<loc>
    Location of the auth cookie (디폴트: data dir)
  * -rpcuser=<user> -rpcpassword=<pw>
    Username for JSON-RPC connections
    Password for JSON-RPC connections
  * -rpcport=<port>
    Listen for JSON-RPC connections on <port> (디폴트: 8332 or testnet: 18332)
  * -rpcallowip=<ip>
    Allow JSON-RPC connections from specified source. Valid for <ip> are a single IP (e.g. 1.2.3.4), a network/netmask (e.g. 1.2.3.4/255.255.255.0) or a network/CIDR (e.g. 1.2.3.4/24). This option can be specified multiple times
  * -rpcthreads=<n>
    Set the number of threads to service RPC calls (디폴트: 4)
* UI Options:

비트코인 코어는 사용자 월렛과 관련된 트랜잭션만을 포함한 데이터베이스를 빌드한다.

* getrawtransaction 명령어 등을 트랜잭션 디코딩 하고 싶다면 configuration 파일에 txindex=1을 설정하고 `bitocind reindex`로 실행한다.

