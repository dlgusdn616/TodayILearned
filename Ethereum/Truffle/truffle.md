# Trrufle All in One

## Migration

Migrations 폴더 내의 파일들은 자바스크립트 파일이며 당신이 Ethereum Network에 컨트랙트를 배포하는 작업을 도와줄 것이다. 이러한 파일들은 컨트랙트를 배포하는 일의 단계를 제대로 나눠주는 역할을 하며, 당신이 배포한 컨트랙트가 차후에 변할 수 있다는 상황을 내포한다.

`$ truffle migrate`프로젝트의 `migrations`폴더내의 모든 파일들에 대해 migrate를 진행할 것이다. 



## TRUFFLE  IMPORT

import할 때, 경로 지정 문제로 애를 많이 먹었다. `./`로 경로가 시작하지 않으면, 트러플은 우리가 진행중인 프로젝트의 `node_modules`폴더를 살피게 된다. 예를 들어, `import "example-library/contracts/SimpleNameRegistry.sol";`과 같은 import문에서는 트러플이 `node_modules`폴더 내에 example-library 폴더부터 살피기 시작할 것이다.

## INTERACTING WEITH YOUR CONTRACTS INTRODUCTION

### READING AND WRITING DATA

* writing data is called a **transaction** where as reading data is called **call**.

### TRANSACTIONS

* Cost gas (Ether)
* Change the state of the network
* Aren't processed immediately
* Won't expose a return value (only a transaction id)

### CALLS

* Are free (do not cost gas)
* Do not change the state of the network
* Are processed immediately
* Will expose a return value

## INTRODUCING ABSTRACTIONS

이더리움 컨트랙트를 자바스크립트로 이용할 수 있는 것이 Contract abstractions이다. 컨트랙트와 쉽게 인터페이스할 수 있게 만든 Wrapper code라고 생각하면 된다. 이로써 우리는 엔진에 대한 이해를 하지 않고도 편하게 사용할 수 있다.  트러플은 이를 위해 자체적으로 [truffle-contract](https://github.com/trufflesuite/truffle-contract) 모듈을 사용한다. 