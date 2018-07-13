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

`msg.sender`이 개발자들 사이에선 선호되고는 한다. 비탈릭은 `tx.origin`의 사용을 피하는 게 좋다고 조언한다. 다음의 글을 참조하자. [Serenity-Proof]("Q&A_How_do_I_make_Serenity-Proof.md")

 

