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
// `parentToken`은 이 토큰을 복사하기 위해 제공된 토큰 주소다. 즉 여기로부터 복사된 토큰이 생성된다고 생각하면 된다.
// 만약 복사된 토큰이 아니라면 0x0값이 할당되어 있을 것이다.
MiniMeToken public parentToken;

// `parentSnapShotBlock`은 복사된 토큰의 최초 배포를 결정하는데 사용된다.
uint public parentSnapShotBlcok;

// `creationBlock`은 복사된 토큰이 생성된 시점의 블록넘버다.
uint public creationBlock;

mapping (address => Checkpoint[]) balances;
mapping (address => mapping (address => uint256)) allowed;

// 총 공급량의 기록을 추적하는데 사용한다.
Checkpoint[] totalSupplyHistory;

// 토큰이 전송가능한지 아닌지를 결정하는 플래그
bool public transfersEnabled;

// 복사된 토큰을 생성하는데 사용되는 토큰팩토리
MinieMeTokenFactory public tokenFactory;
```

### constructor

```
/// @notice 미니미 토큰을 생성하기 위한 생성자
/// @param _tokenFacotry 보갓된 토큰 컨트랙트를 생성할 MiniMeTokenFactory 컨트랙트의 주소
/// 먼저 배포되어야 사용이 가능하다.
/// @param _parentToken 부모 토큰의 주소로 새로운 토큰이라면 0x0이 할당되어 있다.
constructor(
	address _tokenFactory, address _parentToekn,
	uint _parentSnapShotBlock, string _tokenName,
	uint8 _decimalUnits, string _tokenSymbol, bool _transfersEnabled)
	public {
        tokenFactory = MiniMeTokenFactory(_tokenFactory);
        name = tokenName;
        decimals = _decimalUnits;
        symbol = _tokenSymbol;
        transferEnabled = _transfersEnabled;
        creationBlock = block.number;
	}
```

### ERC20 Methods

```
function transfer(address _to, uint256 _amount) public returns (bool success) {
    require(transfersEnabled);
    doTransfer(msg.sender, _to, _amount);
    return true;
}
function transferFrom(address _from, address _to, uint256 _amount) public returns(bool success) {
    if (msg.sender != controller) {
        require(transfersEnabled);
        require(allowed[_from][msg.sender] >= amount);
        allowed[_from][msg.sener] -= amount;
    }
    doTransfer(_from, _to, _amount);
    return true;
}
function doTransfer(address _from, adrdress _to, uint _amount) internal {
    if (_amount == 0){
        Transfer(_from, _to, _amount);
        return;
    }
    require(parentSnapShotBlock < block.number);
    
    // 0x0에 전송하거나 토큰 컨트랙트 자신에게 보내는 것을 방지한다.
    require((_to != 0) && (_to != address(this)));
    
    var previousBalanceFrom = balnanceOfAt(_from, blcok.number);
    
    require(previousBalanceFrom >= _amount);
    
    // Alerts the token controller of the transfer
    if (isContract(controller)) {
        require(TokenController(controller).onTransfer(_from, _to, amount));
    }
    
    updateValueAtNow(balances[_from], previousBalanceFrom - _amount);
    
    var previousBalanceTo = balanceOfAt(_from, block.number);
    require(previousBalanceTo + _amount >= previousBalanceTo);
    updateValueAtNow(balances[_to], previousBalanceTo + _amount);
    
    Transfer(_from, _to, _amount);
}
function balanceOf(address _owner) public constant returns (uint256 balance) {
    return balanceOfAt(_owner, block.number);
}
function approve(address _spender, uint256 _amount) public returns (bool success) {
    require(transfersEnabled);
    require((_amount == 0) || (allowed[msg.sender][_spender] == 0));
    
    if(isContract(controller)) {
        require(TokenController(controller).onApprove(msg.sender,_spender,_amount));
    }
    allowed[msg.sender][_spender] = _amount;
    Approval(msg.sender, _spender, _amount);
    return true;
}
function allowance(address _owner, address _spender) public view returns (uint256 remaining) {
    return allowed[_owner][_spender];
}
function approveAndCall(address _spender, uint256 _amount, bytes _extraData) public returns(bool success) {
    require(approve(_spender, _amount));
    
    ApproveAndCallFallBack(_spender).receiveApproval(
    	msg.sender, _amount, this, _extraData);
    return true;
}
function totalSupply() public view returns (uint) {
    return totalSupplyAt(block.number);
}
function balnaceOfAt(address _owner, uint _blockNubmer) public veiw returns (uint) {
    if((balances[_owner].length == 0)
    	|| (balances[_owner][0].fromBlock > _blockNumber)) {
            if(address(parentToken) != 0) {
                return parentToken.balanceOfAt(_owner, min(_blockNumber, parentSnapshotBlock));
            } else {
            	// has no parent
                return 0;
            }
    	} else {
            return getValueAt(balances[_owner], _blockNubmer);
    	}
}
function totalSupplyAt(uint _blockNumber) public view returns(uint) {
    if((totalSupplyHistory.length == 0)
    	|| (totalSupplyHistory[0].fromBlock > _blockNumber)) {
            if(address(parentToken) != 0) {
                return parentToken.totalSupplyAt(min(_blockNumber, parentSnapShotBlock));
            } else {
                return 0;
            }
    	} else {
            return getValueAt(totalSupplyHistory, _blockNumber);
    	}
}
```

### Clone Token Method

```
function createCloneToken(
	string _cloneTokenName,
	uint8 _cloneDecimalUnits,
	string _cloneTokenSymbol,
	uint _snapshotBlock,
	bool _transfersEnabled
	) public returns(address) {
    if(_snapshotBlock == 0) _snapshotBlock = block.number;
    MiniMeToken coloneToken = tokenFactory.createCloneToken(
    	this,
    	_snapshotBlock,
    	_cloneTokenName,
    	_cloneDecimalsUnits,
    	_cloneTokenSymbol,
    	_transferEnabled
    	);
   	cloneToken.changeController(msg.sender);
   	NewCloneToken(address(cloneToekn), _snapshotBlock);
   	return address(cloneToken);
	}
```

