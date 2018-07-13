# Q&A 정리

## What's the difference between 'msg.sender' and 'tx.origin'?

```solidity
function setOwner() {
      owner = msg.sender;
   }
```

```\
function setOwner() {
      owner = tx.origin;
   }
```

### Answer #1

`msg.sender`는 컨트랙트가 될 수 있다.  EOA 혹은 CA가 될 수 있다.

`tx.origin`은 컨트랙트가 될 수 없다. EOA만 허용된다.

A -> B -> C -> D와 같은 콜체인이 있다고 가정해본다면,  D가 실행될 때 `msg.sender`는 C가 될 것이고, `tx.origin`은 A가 될 것이다.

`msg.sender`이 개발자들 사이에선 선호되고는 한다. 비탈릭은 `tx.origin`의 사용을 피하는 게 좋다고 조언한다. 다음의 글을 참조하자. [Serenity-Proof](Q&A_How_do_I_make_Serenity-Proof.md)

`tx.origin`를 사용하기 전에는 심사숙고하자. 왜냐하면 작성한 컨트랙트는 작성자 본인 뿐만 아니라 많은 사람들에게 이용될 수 있기 때문이다. 그 중에는 당신이 작성한 컨트랙트를 사용자 본인의 컨트랙트 내에서 호출하는 상황도 분명 존재할 수 있다.

위와 같은 상황이라면, 콜체인 중 B, C, D는 각각 추가적인 매개변수를 취해야할 수밖에 없다. A가 가장먼저 A의 주소를 address (`this`)를 B에게 넘겨주고, B는 C에게, C는 D에게 넘기는 구조일 것이다.

사용을 권하지 않는 이유는 콜체인 상 존재하는 B, C, D가 A의 주소가 아닌 다른 값을 끼워서 전송을 할 수 있게 만들어줄 수 있기 때문이다.

 

