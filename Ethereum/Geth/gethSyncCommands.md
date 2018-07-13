# Masering Geth Command

## Sync Command

### 1. Mainnet

* **window**: `geth --syncmode "fast" --cache 4096 --datadir "D:\Ethereum"`
  * D드라이브에 Ethereum폴더를 만들어주고 위의 명령어를 실행
  * 여기서 핵심은 **--datadir** 옵션으로 어떤 폴더를 지정해주느냐에 있다.

### 2. Ropsten

* `geth --testnet --syncmode "fast" --cache 4096 --datadir "D:\Ethereum\Ropsten"`
  * 마찬가지로 **--datadir** 옵션이 핵심이며, **--testnet** 옵션을 줌으로써 **Ropsten** 테스트 네트워크에 싱크시킬 것이라는 것을 알려준다. **rinkeby** 는 별도의 명령어로 구성되어 있다.

### 3. Useful Commands

* **window** 에서 geth를 실행시킨 후에 **geth console**을 별도로 사용하고 싶을 때 사용하는 명령어다. `geth attach "\\.\pipe\geth.ipc"`명령어를 사용함으로써 현재 열려 있는 geth.ipc에 연결한다. 

  > 이 명령어는 mainnnet에 한정된 명령어다. 보통 **geth**를 실행시키면 **geth.ipc**파일이 어디에 생성되는지가 터미널(cmd)에 출력된다. 따라서 해당 경로를 copy해둔 후에 `geth attach`명령어에 활용할 수 있도록 한다.



![Doge](../images/Doge.jpg)
