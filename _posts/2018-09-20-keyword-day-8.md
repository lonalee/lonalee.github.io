---
title: Day 8
layout: post
---
> this, 실행 컨텍스트

## this

### this는 함수 호출 패턴에 따라서 다른 객체에 바인딩되는 참조변수
<br>

* 함수가 호출될 때 내부적으로 arguments객체와 this가 전달됩니다.

<br>
* 함수의 호출방식에 따라 this가 결정된다.
  1. 함수 호출 패턴
  2. 메소드 호출 패턴
  3. 생성자 호출 패턴
  4. apply 호출 패턴

* 함수 호출 패턴
  >전역 함수의 this는 window(Global Object)에 바인딩.<br>
  >내부 함수의 this는 무조건 window에 바인딩. (전역함수의 내부함수이거나, 메소드의 내부함수일 수도 있습니다.)<br>
  >콜백 함수에서의 this는 window에 바인딩<br>

* 메소드 호출 패턴
  >메소드에서의 this는 해당 메소드를 소유하고 있는 (호출하는) 객체에 바인딩.<br>
  >프로토타입 객체도 메소드를 소유할 수 있음. 이 때의 this도 해당 메소드를 소유한 객체에 바인딩 됨<br>

```
var Person = function (name) {
  this.name = name;
}
var lee = new Person('lee');
//Person.prototype === lee.__proto__

Person.prototype.getName = function () {
  return this.name;
}

console.log(lee.getName()); // 여기서의 this는 lee 객체

//prototype 객체에도 name property를 생성해보자.
Person.prototype.name = 'kim';
console.log(Person.prototype.getName()); // 여기서의 this는 Person.prototype 객체
```
* 생성자 호출 패턴

* apply 호출 패턴
  >JS엔진에 의한 this 바인딩이 아니라 사용자에 의해서 임의로 특정 대상에 binding하는 방법



## 실행 컨텍스트

### 실행 컨텐스트는 코드들의 실행 문맥, 실행 순서

 순서라고 해서 위에서 아래로만 이동하는게 아니라 문맥에 따라서 이동하게 되는 것입니다. 브라우저 창 생성 시 windows 객체가 생기고, 런타임 진입전에 변수들이 초기화 단계에 진입하고, 그래서 변수 객체(VO)에 그 변수의 식별자가 기록됩니다. 이러한 호이스팅으로 인해서 변수 선언 이전에 참조가 가능한 것입니다.
일단 window 객체가 생성되고 나서 전역 변수/함수들이 기록되고 나면 실행될 대상들이