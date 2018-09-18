---
title: Day 5
layout: post
---
> Call-by-value VS Call-by-reference, prototype 객체, [[Prototype]] 프로퍼티 vs prototype 프로퍼티


## Call-by-value VS Call-by-reference

  매개변수(parameter)의 개념을 먼저 살펴보자면, 함수 내부에서의 변수 역할을 하는 것을 말합니다. 인자라고 부르기도 하는데, 어떤 함수는 인자값이 불필요한 함수도 있습니다. 함수의 로직 상 값을 가공할 필요가 없는 것들이겠지요. 우리가 함수를 호출할 때 인수(argument)를 전달하면 그 인자값이 파라미터에 담겨 (함수 내에서 특정 변수에 담겨) 그 값들이 가공되는 것입니다.
  >우리가 전달하는 인수가 기본자료형인지 객체형인지에 따라서 함수가 call by value로 동작할지 call by reference로 동작할지 결정됩니다.

  >```call by value는 pass by value처럼 복사본을 생성하는 것입니다. 인수 -> 인자로 넘겨질 때 복사본을 생성해서 전달한다는 의미입니다. 복사본의 전달이기 때문에 값이 전달된 이후의 값의 변경은 서로에게 영향을 주지 않습니다.```

  >```call by reference는 마찬가지로 pass by reference처럼 인수 -> 인자로 넘겨질 때 대상의 참조값을 넘겨줍니다. 그러면 인수, 인자는 모두 같은 참조의 값을 바라보기 때문에 값의 변경은 원본에도 영향을 미칩니다. 우리는 이러한 함수를 비순수 함수라고 부릅니다. 비순수 함수는 일반적으로 복잡성을 증가 시키고 side effect를 발생 시킵니다.```

## prototype 객체

  >객체 생성 방식 (Object 생성자 함수, 생성자 함수, 객체 리터럴)

  >객체를 생성할 때 prototype이 결정됩니다. 이것은 생성되는 객체에 대한 부모 역할을 하는 프로토타입 객체라 불립니다. 이 프로토타입 객체의 프로퍼티와 메소드를 마치 상속 받듯이 사용할 수 있게 됩니다. ```우리가 프로토타입 객체를 다른 객체로 변경할 수도 있습니다```. 이러한 특성을 이용하여 상속의 개념을 의도대로 구현할 수 있습니다.

  >이렇듯 프로토타입 객체는 생성자 함수에 의해서 생성된 각각의 객체의 **공유** 프로퍼티, 메소드를 만들기 위해서 사용되는 것입니다.

  >함수 생성 방식 (함수표현식, 함수선언식, function 생성자 함수)
## [[Prototype]] 프로퍼티 vs Prototype 프로퍼티

  Object 생성자 함수에 의해서 생성된 객체는 'Object.__proto__'과 같은 방법으로 자신의 프로토타입 객체에 접근하고 ~~이를 통해서 constructor에게 접근할 수 있습니다~~.
  >우리가 console.dir을 해보면 [[prototype]] 이라고 표시된 property를 볼 수 있습니다.

  >모든 객체가 갖는 프로퍼티 입니다.<br>
  >객체의 입장(관점)에서 자신의 부모 역할을 하는 프로토타입 객체를 가리키는 프로퍼티입니다.<br>
  >ex.) function.prototype, Object.prototype<br>

  >prototype property

  >함수 객체만 갖는 프로퍼티 입니다. <br>
  >함수 객체가 생성자로 사용될 때 이 객체에 의해 만들어진 객체의 부모 역할을 하는 객체를 가리킵니다. <br>
  >함수가 생성될 때 만들어지며, constructor라는 프로퍼티를 갖는 객체를 가리킵니다. 이 컨스트럭터 프로퍼티는 함수 객체 자신을 가리킵니다. <br>

```
function Person(name) {
  this.name = name;
}

var foo = Person('lee');

console.dir(Person);
console.dir(foo);
Person은 생성자 함수이므로 prototype 프로퍼티를 갖는다.
그러나 foo는 생성자 함수에 의해서 생성된 객체로 prototype 프로퍼티가 없다. ([[prototype]]만 갖는다.)
```
