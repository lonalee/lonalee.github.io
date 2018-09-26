---
title: Day 8
layout: post
---

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
  > 생성자 함수를 사용해서 인스턴스를 만들 때의 순서
    1. 빈 인스턴스의 생성
    2. 빈 인스턴스에 this 바인딩 및 프로퍼티 생성
    3. 생성된 인스턴스의 반환 <br>
      - return 없을 때 : 일반적인 경우로, this에 바인딩되었던 인스턴스가 반환(생성)<br>
      - 다른 것을 return 한다면 : 생성자 함수로써의 역할을 하지 못함<br>

  > 객체 리터럴 방식과 생성자 함수 방식의 차이
두 방식은 각각 다른 프로토타입 객체를 __proto__로 참조하게 됩니다. <br>
객체 리터럴 방식은 내부적으로는 Object 생성자 함수가 객체를 만드는 것입니다. 생성자 함수를 정의해서 인스턴스를 생성하면 새로운 인스턴스의 프로토타입 객체는 생성자 함수.prototype이 될 것입니다.

```
var foo = {
  name: 'jung',
  age: 32
}

var Person = function (age, name) {
  this.name = name;
  this.age = age;
}

var lee = new Person(20, 'lee');
console.dir(foo, lee);      // __proto__: Object, __proto__: Person

```
> 생성자 함수를 new 연산자 없이 호출할 경우, 이 함수는 일반 함수로 간주되어 this는 global에 바인딩됩니다. 인슨턴스가 생성되지 않으며, window에 바인딩 되었기 때문에 프로퍼티는 window에 생성됩니다.

* apply 호출 패턴
  >JS엔진에 의한 this 바인딩이 아니라 **사용자에 의해서 임의로 특정 대상**에 binding하는 방법
apply는 함수가 실행 주체가 됩니다. 이 apply메소드를 호출하는 생성자 함수의 this와 첫번째 인자값이 바인딩되는 것입니다. 그렇게 바인딩한 객체에 해당 프로퍼티를 생성하는데 그 인자값으로 apply 메소드의 두번째 인자값을 사용하는 것이지요. <br>
기본적으로 apply 메소드를 호출하는 주체는 생성자 함수이며, 그 <u>생성자 함수의 this</u>에 메소드의 첫 번째 <u>인자로 전달되는 객체가 바인딩</u> 된다는 것이 중요합니다.
```
var Person = function (name) {
  this.name = name;
}
var foo = {};
Person.apply(foo, ['lee']);
console.log(foo);
```
>apply 사용의 주목적

```
var convertToArray = function () {
  console.log(arguments);

  var arr = Array.prototype.slice.apply(arguments);
  console.log(arr);
}

convertToArray(1,2,3);

var arrTest = [1, 2, 3, 4];
var arrTest1 = arrTest.slice();
console.log(arrTest1);
```

위와 같이 유사배열 객체인 arguments객체를 apply로 호출해서 배열 대상 메소드인 slice를 사용할 수 있습니다.<br>
해석을 해보자면, Array.prototype.slice를 호출하는데, apply 메소드에 의해서 this는 arguments 객체에 바인딩 됩니다.