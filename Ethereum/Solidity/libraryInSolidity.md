# 솔리디티 내에 존재하는 라이브러리들에 대한 재사용성 및 테스트



## 솔리디티 내에서 라이브러리란?

DRY(don't repeat yourself) 원칙을 적용한 코드는 가독성도 좋을뿐만 아니라 유지보수하기에도 용이하다. 그러나 솔리디티 내에서 이런 원칙을 적용하는 것은 다른 언어들에서와 같이 직관적으로 이해가 가지는 않을 것이다.

솔리디티는 코드의 재사용성을 위해 라이브러리 개념을 제공한다.  라이브러리는 제각기 다른 컨트랙트들로부터 호출되고 사용될 수 있다.  객체지향언어를 다뤄봤다면, 라이브러리를 static class내의 static function이라고 생각할 수 있다.

라이브러리들은 컨트랙트와 사실상 같은 개념으로 반드시 사전 배포되어야 사용가능하다. 즉 다른 사용자들이 배포한 라이브러리들은 사용 가능하다는 의미다. 이때 주의할 점은 사용하려는 라이브러리가 이더리움 컨트랙트로 배포되었는지 확인하는 것이다. 존재하지 않는 라이브러리들을 이용하려는 시도를 절대 해서는 안된다.

## 1. 라이브러리 생성하고 사용하기

사람의 이름과 나이를 매핑해주는 자료구조를 유지시켜주는 라이브러리를 생성해볼 것이다. `library`키워드를 사용하여 컨트랙트와 유사한 라이브러리를 생성할 것이다. 라이브러리는 컨트랙트와는 다르게 어떠한 storage variables도 가질 수 없다. 이는 오로지 코드의 재사용성을 위함으로 애초에 라이브러리를 설계할 때 상태 관리는 고려대상이 아니었고 해서는 안된다고 판단했기 때문일 것이다.

```
// Code for StringToUintMap.sol

pragma solidity ^0.4.15;

library StringToUintMap {  
    struct Data {
        mapping (string => uint8) map;
    }

    function insert(
        Data storage self,
        string key,
        uint8 value) public returns (bool updated)
    {
        require(value > 0);

        updated = self.map[key] != 0;
        self.map[key] = value;
    }

    function get(Data storage self, string key) public returns (uint8) {
        return self.map[key];
    }
}
```

위의 라이브러리 코드에는 string과 uint8을 mapping해주는 Data 구조체가 존재하고 mapping 자료구조에 대한 wrapper로서 간단한 insert, get 메서드가 구현되어 있다. 메서드의 인자를 보면, storage 키워드로 Data구조체를 매개변수로 선언한 것을 볼 수 있다. 이는 EVM이 메모리에 Data구조체의 복사본을 저장하지 않고, 단지 Storage로부터 참조로만 받아오게끔 코드를 설계한 것이다.

이번에는 위 라이브러리를 이용하는 컨트랙트 코드를 작성해보자. 

```
// Code for PersonsAge.sol

pragma solidity ^0.4.15;

import { StringToUintMap } from "../libraries/StringToUintMap.sol";

contract PersonsAge {

    StringToUintMap.Data private _stringToUintMapData;

    event PersonAdded(string name, uint8 age);
    event GetPersonAgeResponse(string name, uint8 age);

    function addPersonAge(string name, uint8 age) public {
        StringToUintMap.insert(_stringToUintMapData, name, age);

        PersonAdded(name, age);
    }

    function getPersonAge(string name) public returns (uint8) {
        uint8 age = StringToUintMap.get(_stringToUintMapData, name);

        GetPersonAgeResponse(name, age);

        return age;
    }
}
```

가장 먼저 한 것은 라이브러리를 import하는 것이다. 필자의 경우 코드를 작성할 때 libraries폴더에 별도 저장했으므로 아래와 같이 import 해온다.

`import { StringToUintMap } from "../libraries/stringToUintMap.sol"; `

코드를 보면, 컨트랙트 내에 `StringToUintMap.Data`로 구조체 변수가 생성된 것을 볼 수 있으며 이를 조작하는 두 개의 메서드 `addPersonAge`와 `getPersonAge`를 볼 수 있다.

`addPersonAge`의 경우 name과 age를 받아와서 `StringToUintMap.insert`를 호출하여 `_stringToUintMapData`를 매개변수로 넘긴다. 이때 넘어가는 건, 현재 컨트랙트 내에 있는 `StringToUintMap`인스턴스에 대한 참조값이 넘어가게 된다. 따라서 storage자체는 호출하는 컨트랙트 내에 존재하는 것이다. 그러고 나서 `PersonAdded`이벤트를 호출한다.

`getPersonAge`의 경우 `StringToUintMap.get`을 호출하게 되고 mapping으로부터 사람의 age를 가져온다. 이때도 역시, 구조체의 참조값만 넘어가게 되어 있다. (이는 라이브러리 코드를 작성할 때 storage키워드를 주어 매개변수를 설정하였기에 가능한 것이다.).

위에서 다룬 예제들의 포인트는 컨트랙트를 배포할 때 라이브러리 코드를 먼저 배포하고, 해당 라이브러리의 주소를 컨트랙트 내에 적절히 매핑시켜줘야 한다는 점이다. 이 과정을 linking이라 부른다.

## 2. 라이브러리와 컨트랙트 배포하기

프로젝트 폴더에서 `truffle compile`을 실행하면 `PersonAge.json`파일을 볼 수 있을 것이다. 