# CryptoZombie
#Ethereum

## Stage3
### Chap1: Immutability of Contracts
`코드가 곧 법이다(The code is law).` 는 스마트 컨트랙트의 핵심이다. 한 번 블록체인에 기록된 소스코드는 변하지 않는다. 때문에 코드에 결함이 발견되어도 이미 등록되어 있는 코드를 수정할 수 있는 방법은 존재하지 않는다.

#### External dependencies
하드 코딩이 되어 있는 상태에서 코드 자체에 결함이 발생했을 경우 수정할 수 있는 방안은 존재하지 않는다. 그래서 코드 내에 의존성이 존재하는 경우, 의존하는 컨트랙트의 주소를 변경할 수 있게끔 설정해두는 것이 중요하다.
`setKittyContractAddress` 와 같이 대비책을 만들어 두어 만일의 상황에 대비할 수 있도록 한다.

## Chap2: Ownable Contracts
### OpenZeppelin's Ownable contract
OpenZeppelin 솔리디티 라이브러리를 활용하여 Ownable을 사용해본다. 

## Chapter 10: Saving Gas With 'View' Functions
### View functions don't cost gas
external로 실행될 때만 가스비가 들지 않는다. 트랜잭션을 발생시키지 않기 때문이고, 이는 자신의 로컬 노드에서 데이터만 불러오는 작업만 수행한다. 그러나 만약 view funciton이 internal로 동작한다면 이는 가스비를 발생시킬 수 있다.
view가 아닌 함수에서 데이터를 교환하는 로직 후에 view 함수를 호출했을 경우, 이는 EVM에서의 검증 및 실행이 필요하기 때문이다 