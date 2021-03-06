---
title: Day 3
layout: post
---
# pass-by-value vs pass by reference, object property vs method, hoisting

## pass-by-value vs pass by reference

값에 의한 전달은 변수가 값을 전달하는 방식, 참조에 의한 전달은 객체가 값을 전달하는 방식입니다. 먼저 값에 의한 전달 방식을 알아보자면, 변수 A의 값을 변수 B로 할당할 때 A의 값에 대한 복사본이 생성되고 그 복사본(value)이 전달되는 방식입니다. 변수 B에 변수 A 자체를 할당하는 것처럼 보일지라도 A의 값의 복사본을 넘겨주는 (pass) 것이기 때문에 A와 B는 다른 곳에 있는 값들을 바라보게 됩니다. 이것은 서로의 값에 대한 영향을 주지 못한다는 얘기가 됩니다. 이와 같은 이유로 객체를 제외한 기본자료형들은 변경 불가능한 값을 갖게 되는 것입니다.
참조에 의한 전달 방식은, 객체 A의 어떤 프로퍼티 값을 B 객체의 프로퍼티에 할당한다면 A 객체의 참조값(reference)을 직접 넘겨주기 (pass) 때문에 두 객체는 같은 값을 바라보게 됩니다. 이와 같은 이유로 객체는 변경 가능한 (mutable)한 값이 됩니다. 즉 A,B 모두에서 객체의 값을 변경할 수 있다는 것입니다. 이는 의도하지 않는 값의 변경과 같은 문제를 야기할 수도 있습니다.

## object property vs method

객체는 유사한 성격을 갖는 값들의 집합이며 이 값들에 대한 특정 행위를 하기 위한 함수도 포함할 수 있습니다. 한 개인을 표현하기 위한 객체가 있다면 성명, 나이, 직업, 주소와 같은 데이터 (property key & value)를 하나의 객체에 담을 수 있으며 이 데이터를 처리하기 위한 함수를 어떤 프로퍼티 안에 담아서 정의하면 그것을 method라고 부를 수 있습니다.

## hoisting

자바스크립트에서 모든 선언문은 호이스팅이 됩니다. 이는 웹 어플리케이션의 런타임 전에 JS엔진이 코드 내에 모든 var 선언문을 읽어서 변수 생성의 3단계 중 초기화 단계까지 진행하기 때문입니다. 변수는 선언 -> 초기화 -> 할당 단계를 거쳐 생성되는데 var 변수는 선언과 동시에 undefined로 초기화 됩니다. 따라서 호이스팅된 변수는 해당 변수의 scope의 최상단으로 이동한 것처럼 참조가 가능하다는 것입니다. 함수는 선언식일 경우 전체가 호이스팅 되어서 정상적으로 함수의 실행이 가능합니다.