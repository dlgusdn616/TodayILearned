# Basic Web3
#Ethereum/web3/commands

check npm version: `npm list web3`

getBalance to Ether
`web3.fromWei(eth.getBalance(), "ether")`

## web3@0.20.6
working with ganache. 
see ganache setting -> and then port number will be 8545 default
```
Web3 = require('web3')

provider = new Web3.providers.HttpProvider("http://localhost:8545")
Web3 = new Web3(provider)
```

check accounts balance
```
web3.eth.accounts.forEach(account => {
	balance = web3.eth.getBalance(account);
	console.log(balance);
})
```

```
from = web3.eth.accounts[0]
to = web3.eth.accounts[1]
transaction = { from: from, to: to, value: web3.toWei(10, "ether") }
transactionHash = web3.eth.sendTransaction(transaction)
```

## solc를 이용한 스마트 컨트랙트 배포
#Ethereum/solc

`npm install -g solc`
배포하기 원하는 컨트랙트 소스코드를 `MyToken.sol` 형식으로 저장한다.

`solcjs MyToken.sol --bin`
위 명령어로 컴파일러는 `MyToken_sol_MyToken.bin` file을 생성할 것이다. 이 파일은 bytecode를 포함하고 있다는 것을 알 수 있다. 

`solaces MyToken.sol --abi`
위 명령어로 소스코드를 통해 ABI를 빌드한다. 계약과 상호작용하기 위해 필요한 인터페이스 및 템플릿이다. 
명령어로 생성된 파일은 `MyToken_sol_MyToken.abi` 이고, 파일의 형식은 JSON이다. 

Ganache 실행과 함께 아래의 명령어로 node를 테스트해보도록 한다.
```
Web3 = require('web3')
provider = new Web3.providers.HttpProvider("http:localhost:8545")
web3 = new Web3(provider)
```

Web3는 컨트랙트 ABI를 파싱하여 JavaScriptAPI와 함께 상호작용할 수 있게끔 만들어준다. 따라서 우리가 필요한 건 배포에 필요한 바이트코드이고 이 과정을 통해 컨트랙트의 새로운 인스턴스를 배포하면 된다.

```
// load files
myTokenABIFile = fs.readFileSync('MyToken_sol_MyToken.abi')
myTokenABI  = JSON.parse(myTokenABIFile.toString())
myTokenBINFile = fs.readFileSync('MyToken_sol_MyToken.bin')
myTokenByteCode = myTokenBINFile.toString()
//deploy
account = web3.eth.accounts[0]
MyTokenContract = web3.eth.contract(myTokenABI)
contractData = { data: myTokenByteCode, from: account, gas: 999999 }
deployedContract = MyTokenContract.new(contractData)
```

배포가 잘 되었는지 `deployedContract.address`  명령어로 확인해본다.

인스턴스를 생성하여 컨트랙트를 활용해보자.
```
contractAddress = deployedContract.address
instance = MyTokenContract.at(contractAddress)

web3.eth.accounts.forEach(address => {
 tokens = instance.balanceOf.call(address)
 console.log(address + ": " + tokens)
})
```

