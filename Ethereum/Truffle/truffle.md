# Trrufle All in One

## Migration

Migrations 폴더 내의 파일들은 자바스크립트 파일이며 당신이 Ethereum Network에 컨트랙트를 배포하는 작업을 도와줄 것이다. 이러한 파일들은 컨트랙트를 배포하는 일의 단계를 제대로 나눠주는 역할을 하며, 당신이 배포한 컨트랙트가 차후에 변할 수 있다는 상황을 내포한다.

`$ truffle migrate`프로젝트의 `migrations`폴더내의 모든 파일들에 대해 migrate를 진행할 것이다. 

## INITIAL MIGRATION

트러플은 마이그레이션 컨트랙트를 제공해준다. 컨트랙트는 반드시 특정한 인터페이스를 포함하고 있어야 한다. 대부분의 프로젝트에서 contract Migrations은 첫번째로 배포된 이후에 따로 업데이트 되거나 하지 않는다. 



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

## USING TRUFFLE DEVELOP AND THE CONSOLE

* Truffle console: 이더리움 클라이언트와 상호작용할 수 있는 기본적인 콘솔
* Truffle Develop: An interactive console that also spawns a development blockchain 

### WHY TO DIFFERENT CONSOLE?

* Truffle console:
  * Ganache or geth를 이미 사용중일 것이다.
  * 테스트넷 혹은 메인넷으로 migrate하고 싶을 것이다.
  * 특정 mnemonic 또는 계정 리스트를 원할 것이다.
* Truffle Develop:
  * 프로젝트를 배포하지 않고 테스트 할 수 있다.
  * 특정 어카운트를 지정할 필요가 없다.
  * 별도의 블록체인 클라이언트를 설치할 필요가 없다.

### COMMANDS

#### CONSOLE

`truffle console`

이 명령어는 설정 안에 네트워크를 정의한 `development`를 먼저 살펴볼 것이고 연결이 가능하다면 연결할 것이다. 이 명령어는 `--network <name>` 옵션과 함께 사용할 수 있다. 자세한 내용은 [truffle-network](https://truffleframework.com/docs/advanced/networks) 섹션과 [command-reference](https://truffleframework.com/docs/advanced/commands)을 참조하도록 한다.

해당 명령어를 실행시키면 `$truffle(development)>`라는 문구가 뜰 것이고, 현재  `development`네트워크를 이용하고 있다고 표시해줄 것이다.

#### TRUFFLE DEVELOP

`truffle develop`

개발용 블록체인을 9545포트에 런칭시킬 것이다. `truffle.js`의 내용은 읽어오지 않는다. 아래와 같은 결과창이 나올 것이다.

```
Truffle Develop started at http://localhost:9545/

Accounts:
(0) 0x627306090abab3a6e1400e9345bc60c78a8bef57
(1) 0xf17f52151ebef6c7334fad080c5704d77216b732
(2) 0xc5fdf4076b8f3a5357c5e395ab970b5b54098fef
(3) 0x821aea9a577a9b44299b9c15c88cf3087f3b5544
(4) 0x0d1d4e623d10f9fba5db95830f7d3839406c6af2
(5) 0x2932b7a2355d6fecc4b5c0b6bd44cc31df247a2e
(6) 0x2191ef87e392377ec08e7c08eb105ef5448eced5
(7) 0x0f4f2ac550a1b4e2280d04c21cea7ebd822934b5
(8) 0x6330a553fc93768f612722bb8c2ec78ac90b3bbc
(9) 0x5aeda56215b167893e80b4fe645ba6d5bab767de

Private Keys:
(0) c87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3
(1) ae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f
(2) 0dbbe8e4ae425a6d2687f1a7e3ba17bc98c673636790f1b8ad91193c05875ef1
(3) c88b703fb08cbea894b6aeff5a544fb92e78a18e19814cd85da83b71f772aa6c
(4) 388c684f0ba1ef5017716adb5d21a053ea8e90277d0868337519f97bede61418
(5) 659cbb0e2411a44db63778987b1e22153c086a95eb6b18bdf89de078917abc63
(6) 82d052c865f5763aad42add438569276c00d3d88a2d062d36b2bae914d58b8c8
(7) aa3680d5d48a8283413f7a108367c7299ca73f553735860a87b08f39395618b7
(8) 0f62d96d6675f32685bbdb8ac13cda7c23436f63efbb9d07700d8669ff12b7c4
(9) 8d5366123cb560bb606379f90a0bfd4769eecc0557f1b362dcae9012b548b1e5

Mnemonic: candy maple cake sugar pudding cream honey rich smooth crumble sweet treat
```

> Note: 니모닉과 주소들은 변하지 않는다. 만약 다른 니모닉과 주소들을 사용하고 싶다면, Ganache를 사용할 것을 추천한다. 이 주소들과 니모닉은 절대로 메인넷에서 사용하지 않도록 한다.

### FEATURES

Truffle Develop이나 console 둘 다에 적용되는 내용이다.

* 컴파일된 모든 컨트랙트는 바로 사용될 수 있다.

* 커맨드(예를 들면 `migrate --reset`) 입력 후에는 컨트랙트가 개정될 것이고, 사용자는 새롭게 부여된 주소와 바이너리들을 즉시 사용할 수 있다. (변경 내용이 바로 적용된다는 의미.)

* `web3`라이브러리를 이용할 수 있으며 이더리움 클라이언트에 연결되게끔 설정되어 있다.

* promise를 리턴하는 모든 커맨드들은 자동으로 해결될 것이다. 아래와 같이 `.then()`을 사용하지 않고도 간단한 커맨드를 작성할 수 있다.

  `MyContract.at("0xabcd...").getValue.call();`

  위 명령어는 아래와 같이 리턴할 것이다.

  `5`

  

### 가능한 커맨드들

- `build`
- `compile`
- `create`
- `debug`
- `exec`
- `install`
- `migrate`
- `networks`
- `opcode`
- `publish`
- `test`
- `version`



## USING THE BUILD PIPELINE

트러플 1.0과 2.0이 웹 애플리케이션을 위한 기본적인 빌드 시스템이 되었다. 트러플 3.0으로 오면서 이 빌드시스템은 별도의 모듈로 분리되었다. 이 말인 즉슨, artifacts를 HTML, Javascript and CSS로 바꿔줄 수 있다는 의미다.

기본 빌더에는 신속하게 시작할 수있는 몇 가지 표준이 있다.

- Automatic bootstrapping of your application within the browser, importing your compiled contract artifacts, deployed contract information and Ethereum client configuration automatically.
- Inclusion of recommended dependencies, including [web3](https://github.com/ethereum/web3.js/tree/master) and [Ether Pudding](https://github.com/ConsenSys/ether-pudding).
- Support for ES6 and JSX built-in.
- SASS support for manageable CSS.
- And UglifyJS support for creating minified versions of your Javascript assets.

디테일한 설정법은 다음의 링크를 참조. [build-config](https://github.com/trufflesuite/truffle-default-builder/tree/master)

별도의 빌드 프로세스를 설정하지 않는다면 `$ truffle build`는 에러를 반환할 것이다.

### RUNNING AN EXTERNAL COMMAND

트러플이 빌드할 때마다 외부의 커맨드를 실행시키고 싶다면, 다음과 같이 간단한 옵션을 string으로 프로젝트 설정 파일에 넣어주자.

```
module.exports = {
  // This will run the `webpack` command on each build.
  //
  // The following environment variables will be set when running the command:
  // WORKING_DIRECTORY: root location of the project
  // BUILD_DESTINATION_DIRECTORY: expected destination of built assets (important for `truffle serve`)
  // BUILD_CONTRACTS_DIRECTORY: root location of your build contract files (.sol.js)
  //
  build: "webpack"
}
```

### PROVIDING A CUSTOM FUNCTION

```
module.exports = {
  build: function(options, callback) {
     // Do something when a build is required. `options` contains these values:
     //
     // working_directory: root location of the project
     // contracts_directory: root directory of .sol files
     // destination_directory: directory where truffle expects the built assets (important for `truffle serve`)
  }
}
```

### CREATING A CUSTOM MODULE

```
var DefaultBuilder = require("truffle-default-builder");
module.exports = {
  build: new DefaultBuilder(...) // specify the default builder configuration here.
}
```

