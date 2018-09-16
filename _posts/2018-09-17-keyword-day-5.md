---
title: Day 5
layout: post
---
# Call-by-value VS Call-by-reference, prototype 객체, [[Prototype]] 프로퍼티 vs prototype 프로퍼티

## Call-by-value VS Call-by-reference

## prototype 객체
  객체 생성 방식 (Object 생성자 함수, 생성자 함수, 객체 리터럴)
  Object 생성자 함수 사용하면 Object.prototype 객체가 생성됩니다. 이것은 생성되는 객체에 대한 프로토타입 객체라고 불리는 것으로 생성된 객체가 Object 생성자 함수의 프로퍼티에 접근할 수 있도록 도와줍니다.

## [[Prototype]] 프로퍼티 vs prototype 프로퍼티
  Object 생성자 함수에 의해서 생성된 객체는 'Object.__prototype__'과 같은 방법으로 자신의 프로토타입 객체에 접근하고 이를 통해서 constructor에게 접근할 수 있습니다.
  반대로 constructor는 'Object.prototype'과 같은 방식으로 프로토타입 객체에 접근할 수 있습니다.