---
title: Day 6
layout: post
---
> navtive object VS Host object, Static Method VS Prototype Method, 가변인자함수


## Navtive object VS Host object

자바스크립트에서의 객체는 3가지 종류가 있습니다. (native, host, user-defined)

1. Native Objects는 Built-in / Global Objects*(Host Object를 뜻하는 Global Object(Window, Global)과는 다름, **혼동 주의**)*라고도 불리우는데, ECMAscript에 정의된 객체, 어플리케이션 전역에서 공통된 기능을 제공하는 객체, 어플리케이션 환경과 관계없이 사용할 수 있는 객체입니다.
> Native Object에는 생성자 함수 객체와 그와 관련된 메소드가 포함됩니다.<br>
> 예를 들어, Objcet, Function, Array, String 등이 있습니다. 모두 생성자 함수이지요. 이들과 new 연산자를 사용해서 인스턴스를 만들 수 있고 이것의 type은 생성자 함수의 그것과 같게 됩니다.<br>
> 각각의 Native Object에는 우리가 흔히 사용하는 메소드들이 포함되어 있습니다. 우리가 이들 객체로 인스턴스를 생성하게 되면 프로토타입 체이닝에 의해서 이들의 프로토타입 메소드를 생성된 객체에서도 이용 가능하게 되죠.<br>
>> **example.**<br>
>> const arr = new Array('lee', 'kim', 'jung', 'lona');<br>
>> const arr1 = ['lee', 'kim', 'jung', 'lona'];   // 위의 방식과 동일, syntactic sugar<br>
>> console.log(arr.slice(1));<br>
<br>
> 예외적으로 기본자료형에서도 built-in 객체의 메소드를 사용할 수가 있습니다. 이것은 조금은 우리를 혼란스럽게 하기도 합니다.;;<br>
<br>
>> **example.**<br>
>> const string1 = 'I am a string';<br>
>> const string2 = new String('I am also a string');<br>
>> console.log(typeof string1);    //String<br>
>> console.log(typeof string2);    //String<br>
>> console.log(string1.toUpperCase());<br>
>> console.log(string2.toUpperCase());     //결과 동일<br>
<br>
  > 위와 같이 기본자료형인 string에서도 String built-in Object의 메소드를 사용 가능합니다. <br>
  > 기본자료형은 객체가 아니죠. 당연히 프로토타입 체인의 개념도 없습니다만, 위와 같이 동작하는 이유는 wrapper 객체라는 개념이 존재하기 때문입니다. <br>
기본자료형으로 built-in Object의 메소드를 호출하면 일시적으로 그 기본자료형은 wrapper 객체로 변환됩니다. 따라서 위와 같이 동작할 수 있는 것입니다.
wrapper객체는 String, Number, Boolean이 있습니다.

2. Host Object는 특정 환경에서 제공하는 객체로 대표적으로는 브라우저 환경과 서버 환경에서 각각 제공하는 Global Object가 존재합니다. BOM, DOM도 있습니다.
> BOM은 브라우저 탭 또는 창의 모델링을 담당하고 대표적으로 document라는 객체가 있습니다. 현재 로드된 웹페이지에 대한 전체 표현을 담당하죠. 이 document 객체는 다시 DOM의 최상위 객체로 페이지 상의 여러 HTML 요소들에 대한 기능을 담당합니다.
> 브라우저 환경을 먼저 알아보자면 우리가 브라우저를 열게 되면 (어플리케이션은 runtime 진입 전) 유일한 최상위 객체인 window 라는 Global Object가 생성됩니다. 이것이 각 환경에서 존재하는 host object입니다.
> Window는 전역 객체이므로 전역 스코프를 갖습니다. 한마디로 전역 변수, 전역 함수를 거느릴 수 있는 것이죠.<br>
> 전역 변수는 전역 객체 window의 프로퍼티로 존재하게 되고 전역 함수는 메소드로 존재하게 됩니다. 이것은 **실행 컨텍스트**와도 관련이 있는데, 간단히 짚고 넘어가자면 어플리케이션 런타임 이전에 JS엔진이 전체 코드를 1차적으로 훑어 봅니다.<br> 이 때 전역 변수, 표현식으로 선언된 함수들은 (변수) 호이스팅 되어서 전역 객체의 변수 객체에 담기게 되는 것입니다. 이러한 흐름으로 전역 변수, 전역 함수는 전역 객체의 프로퍼티, 메소드가 됩니다.
> built-in Object도 window의 자식 객체입니다. 다만 전역 객체의 자식 객체를 사용할 때, 전역 객체명의 기술은 생략 가능하기 때문에 우리가 window를 명시하지 않아도 되는 것이죠.<br>
> server-side에서는 global이 전역 객체로 존재하게 됩니다.

## Static Method VS Prototype Method

1. Static Method는 인스턴스를 생성하지 않고서도 우리가 사용할 수 있는 메소드입니다. 대표적으로 Math 객체가 갖고 있는 메소드들이죠.
> Static Method를 호출할 때의 형식에 주목하고자 합니다. 그저 Math객체가 갖고 있는 메소드를 부르고 인자값을 넣기만 하면 됩니다. 이것이 시사하는 바는 특정 인스턴스에서 동작하는 메소드가 아니라는 점이죠. 그런 것들이 바로 Static Method입니다.<br>
>>const arr = [1,2,3,4]<br>
>>console.log(Math.max(...arr));    //spread 연산자



## 가변인자함수
