---
title: Day 9
layout: post
---

## 실행 컨텍스트 (중요)

### 실행 컨텐스트(EC)는 실행가능한 코드들의 실행 환경, 코드들을 형상화하고 구분하는 추상적 개념

EC도 결국에는 객체의 형태로 존재합니다. 이를 구성하는데에 3가지의 프로퍼티가 있고 이 또한 객체로 존재하지요.<br>
다음의 3가지 프로퍼티는 전역 EC를 포함한 모든 EC에 존재합니다.

1. **Variable Object** (이하 VO)
    1. 변수
    2. parameter / arguments
    3. 함수 선언식
   > 위와 같은 정보를 VO에 담게 됩니다.
2. **Scope Chain** (이하 SC)
3. **this Value** (이하 this)
이상.<br>

EC의 프로퍼티들은 **값으로 존재하기 때문에 특정 객체를 포인팅**하고 있다고 합니다. 이들은 전역 EC일 때와 함수 EC일 때 각각 대상이 다릅니다.<br>

>전역 EC에서는 VO -> GO 입니다. 이로 인해서 전역 변수/함수는 GO의 프로퍼티로 존재하게 됩니다. GO는 모든 객체의 최상위에 존재하지요.

>함수 EC에서는 VO -> AO (Activation Object) 이며 지역변수 및 내부 함수를 AO에 담게 됩니다. AO에는 arguments 객체가 추가되어 매개변수, 인수에 대한 정보를 담게 됩니다.

1. **Variable Object** (이하 VO)
2. **Scope Chain** (이하 SC)
    1. GO / AO의 리스트를 가리킴
    2. 프로토타입 체인과의 차이점
3. **this Value** (이하 this)

Scope Chain(SC)은 상위 scope을 참조하기 위한 레퍼런트 리스트 입니다. foo() 와 bar()의 예시를 다시 보면, 전역 EC -> foo EC -> bar EC 순서로 스택상에 생성이 됩니다. 한 단계씩 거치면서 SC가 1씩 증가한다고 생각하면 이해가 쉽겠습니다.<br>

1. GEC = 0 **(GO)**
2. foo EC = 0, 1 **(GO)**
3. bar EC = 0, 1 **(foo AO)**, 2 **(GO)**

이들이 객체 형태로 저장되어 있는것이지요. 항상 0은 자신의 AO를 참조하는 값이라 생각합시다. <br>
정리하자면, 내부함수인 bar가 foo를 참조하기 위해서는 SC상에서 한 단계 올라가야 하기 때문에 1의 위치에서 foo의 AO를 참조하고 있을 겁니다. bar의 scope 영역에서 그렇다는 말이죠.<br>
이렇게 scope chain이 존재하기 때문에 **내부함수에서 상위 함수 및 전역을 참조 가능**한 것입니다.<br>
**참조 순서 0 -> 1 -> 2** <br>
SC는 변수를 참조하기 위하여 존재하고 상위 객체의 프로퍼티(메소드 포함)를 참조하기 위하여 프로토타입 체인이 존재합니다.

예를 들어 확인해 봅시다.
```
var x = 'xxx';

function foo () {
  var y = 'yyy';              //  지역변수 yyy

  function bar () {           //  내부함수 bar
    var z = 'zzz'
    console.log(x + y + z);   // 자신의 외부함수의 y 참조 가능, global까지 scope chain에 의해서 참조 가능
  }

  bar();
}
foo();
```

1. global Execution Context 의 생성 (in STACK)
    * 어플리케이션의 종료 시까지 존재함
    * 전역 코드로 어플리케이션의 제어권이 넘어오면 GEC가 생성됨
2. foo() EC 생성
3. bar() EC 생성
4. bar EC의 소멸
5. foo EC의 소멸

* 각 EC에서는 다음과 같은 처리가 발생합니다.

1. SC 생성 및 초기화
2. Variable Instantiation(변수 객체화)
3. this value 결정


이 중에서 변수 객체화에 대해 살펴 봅니다.
일단 변수 객체화는 VO에 값을 setting하는 과정이라 생각하면 됩니다. 위에서 살펴본 바와 같이, VO는 EC가 GEC인지, 함수EC인지에 따라서 참조대상이 GO냐 AO냐가 결정됩니다. <br>
먼저 함수EC에서는