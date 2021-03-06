---
title: Day 4
layout: post
---
# Mutable VS Immutable, First-class-object, callback function

## Mutable VS Immutable

자바스크립트에서 기본자료형은 immutable, 변경 불가능한 자료형입니다. 물론 재할당은 가능합니다. 하지만 이것은 메모리 상의 다른 공간에 새로이 저장되는 것 입니다. 이러한 성질은 pass by value의 개념으로 설명됩니다. (Day 3 참고)
mutable한 기본자료형은 다른 변수에 할당된다면 기존 값의 변경이 새로운 변수에게 영향을 주지 않습니다. pass by value이기 때문입니다.

기본자료형 이외의 객체형은 (배열 포함) mutable한 성질을 갖습니다. pass by reference의 개념이 적용됩니다.
따라서, 객체형이 다른 변수에 전달(pass)된다면 모두 같은 곳을 참조하기 때문에 값의 변경이 모두에게 영향을 줍니다.
변경 가능 여부 (pass by value / reference 중 어떤 방식이 적용되는지)는 전달하는 값이 기본자료형인지 객체형인지에 따라 결정됩니다.

## First-class-object

자바스크립트에서 대표적인 일급 객체는 함수입니다. 일급 객체는 익명 리터럴로 표현 가능, 특정한 값으로써 역할 가능한 것이라고 간단히 설명 가능합니다.
특정한 값으로써 역할을 한다는 것은 다른 함수에 인자로 전달할 수 있고, 리턴 값으로도 사용될 수 있는 것을 의미합니다. 이것은 프로그래밍에서의 기본적 조작, 즉, 생성, 대입, 연산 등을 할 수 있다는 의미가 됩니다.

## callback function

callback 함수를 이해하기 위해서는 비동기 처리 방식을 알아야 합니다. 비동기 처리란 자바스크립트가 동작할 때 코드의 실행 결과를 기다리지 않고서도 다음 코드를 실행할 수 있는 동작 방식을 의미합니다.
자바스크립트의 대부분의 DOM 이벤트와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기식 처리 모델로 동작합니다. 대표적인 예로 setTimeout이 있는데, 일정 시간이 경과한 후에 동작할 함수를 정의할 수 있습니다. 이와 같은 함수가 바로 callback 함수 입니다. Ajax 요청에서는 서버로부터의 응답을 기다리지 않고 다음의 코드를 실행하여 화면을 표시할 수도 있습니다. 그렇게 되면 우리가 기대한 서버의 결과를 누락 시키고 다음 코드를 실행할 수도 있습니다. 이를 방지하기 위해서도 callback 함수가 사용됩니다. 서버의 응답이 수신되면 그 때 callback함수로 해당 처리를 하게 만드는 것입니다.
