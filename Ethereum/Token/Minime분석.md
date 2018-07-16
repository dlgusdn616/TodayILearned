## TokenController_all

토큰 컨트롤러는 반드시 아래와 같은 인터페이스를 구현해야 한다.

```
@notice _owner가 Minime Token Contract에게 ether를 전송할 때 호출된다.
@param _owner: 토큰을 만들기 위해 이더를 전송한 address다.
@return ether가 수납되었다면 true를 그렇지 않다면 false를 리턴한다.
function proxyPayment(address _owner) public payable returns(bool);
```

```
@notice 컨트롤러가 원할때 반응할 수 있도록 토큰 전송에 대해 컨트롤러에게 알린다.
@param _from 전송이 발생된 address
@param _to 전송의 목적지 address
@param _amount 전송할 양
@return 컨트롤러가 전송을 승인해주지 않으면 false를 리턴
fucntion onTransfer(address _from, address _to, uint _amount) public returns(bool);
```

```
@notice 컨트롤러가 원할때 반응할 수 있도록 승인에 대해 컨트롤러에게 알린다.
@param _owner `approve()`를 호출할 address
@param _spender `approve()`호출에 있는 spender
@param _amount `approve()`호출에 있는 토큰의 양
@return 컨트롤러가 approval를 승인해주지 않으면 false를 리턴
function onApprove(address _owener, address _spender, uint _amount) publice returns(bool);
```

## Controlled

```
modifier onlyController { require(msg.sender == controller); _; }

address public controller;

function Controlled() public { controller = msg.sender; }

@notice 컨트랙트의 컨트롤러를 바꿔준다.
@param _newControllelr 컨트랙트의 새로운 컨트롤러
function changeController(address _newController) public onlyController {
    controller = _newController;
}
```



## ApproveAndCallFallback

```
function receiveApproval(address from, uint256 _amount, address _token, bytes _data) public;
```



## MiniMeToken is Controlled

```
string public name;			// The Token's name: e.g. DigixDAO Tokens
uint8 public decimals;		// Number of decimals of the smallest unit
string public symnbol;
string public version = 'MMT_0.2';	// 버전 표기
```

```
struct Checkpoint {
    uint128 fromBlock;
    uint128 value;
}
```

```
MiniMeToken public parentToken;
uint public parentSnapShotBlcok;
uint public creationBlock;

```

