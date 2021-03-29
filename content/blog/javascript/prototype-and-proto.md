---
title: '[[Prototype]], __proto__과 prototype'
date: 2021-3-24 23:53:13
category: 'javascript'
draft: false
---

> 자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.

ECMA-262에서 prototype은 'object that provides shared properties for other objects'이라고 설명되어있다.

p257

모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 갖는다. `[[Prototype]]` 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

`__proto__` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다. 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다. `[[Prototype]]` 내부 슬롯에도 직접 접근할 수 없으며 `__proto__` 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있다.

prototype 프로퍼티는 생성자 함수로 호출할 수 있는 객체, 즉 constructor만이 소유하는 프로퍼티다. 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

<br />

**참고**

<div style="font-size: 12px;">

- 모던 자바스크립트 Deep Dive, 이웅모 (2020)

- <a href="https://262.ecma-international.org/6.0/" target="_blank">ECMAScript 2015 Language Specification</a>

- <a href="https://medium.com/jspoint/what-are-internal-slots-and-internal-methods-in-javascript-f2f0f6b38de" target="_blank">What are “Internal Slots” and “Internal Methods” in JavaScript?</a>

</div>
