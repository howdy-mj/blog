---
title: '왜 함수의 타입은 object가 아닌 function을 반환할까?'
date: 2021-4-10 00:00:00
category: 'javascript'
draft: false
---

자바스크립트에는 크게 <span class="definition">원시 타입(Primitive Type)</span>, <span class="definition">참조 타입(Reference Types)</span>으로 분류된다. 내가 갖고 있는 데이터가 정확히 어떤 타입인지 알고 싶으면 `typeof` 메서드를 사용하는데, 이때 원시 타입은 그대로 타입이 반환 되지만 참조 타입은 그렇지 않았다.

맨 처음 자바스크립트를 공부했을 때 가장 의아했던 점 중 하나였다.

가령 배열의 `map()`을 돌려야해서, 배열로 잘 들어왔는지 확인해보고싶어서 `typeof`를 사용했지만, 결과는 <span class="return">object</span>로 나올 뿐이었다. 조금 공부하다보니, 자바스크립트의 참조타입은 모두 객체이기 때문에 <span class="return">object</span>라 반환되었던 것이었다. 때문에 MDN에서도 이를 참조 타입이 아닌 <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures" target="_blank" class="link">객체(Objects)</a>라 정의하고 있는 것 같다.

하지만 그 중에서도 함수만은 달랐다. 그 어떠한 함수의 형태를 쓰더라도 항상 <span class="return">function</span>이 반환됐다.

```js
typeof [] // output: 'object'
typeof {} // output: 'object'
typeof new Date() // output: 'object'
typeof function() {} // output: 'function'
```

처음에는 function 키워드 때문일거라 생각을 했지만, function 키워드를 쓰지 않는 화살표 함수 역시 타입이 <span class="return">function</span>으로 나왔다.

```js
const arrFunc = () => {}
typeof arrFunc // output: 'function'
```

그리고 생성자 함수에 의해 만들어진 객체도 함수만 타입이 <span class="return">object</span>가 아닌 <span class="return">function</span>으로 반환된다.

```js
typeof new String('a') // output: 'object'
typeof new Number(123) // output: 'object'
typeof new Boolean(true) // output: 'object'
typeof new Array(1, 2, 3) // output: 'object'
typeof new Object() // output: 'object'
typeof new Function('x', 'return x') // output: 'function'
```

<div style="font-weight: bold; font-style: italic;">도대체 왜 자바스크립트에서 함수만 타입을 제대로 반환해주는 걸까?</div>

<br >

이를 대답하기 위해서 다시 한 번 강조해야 할 것은, 함수도 객체라는 점이다. 그리고 자바스크립트에서 모든 객체는 <span class="definition">내부 슬롯(internal slot)</span>과 <span class="definition">내부 메서드(internal method)</span>를 갖는다.

### 내부슬롯과 내부 메서드

<a href="https://262.ecma-international.org/11.0/#sec-object-internal-methods-and-internal-slots" target="_blank">ECMAScript 262</a>에 따르면, 내부 슬롯과 내부 메서드는 자바스크립트의 구현을 위해 존재하며, 개발자들이 직접 접근할 수 없다.

내부 메서드는 다형성을 띄우며, 각기 다른 객체 값이 다른 알고리즘을 수행할 수 있으며, 만약 해당 객체에서 지원하지 않는 메서드를 실행할 경우 **TypeError**를 반환한다.
내부 슬롯은 객체와 연결되고 다양한 ECMAScript 규격 알고리즘에 사용되는 내부 상태(state)이다. 내부 슬롯은 객체 프로퍼티가 아니며, 상속되지 않는다. 또한 명시되지 않는한, 내부 슬롯은 객체를 생성하는 과정 중 하나이며 동적으로 추가될 수 없으며, 초기 값은 **undefined**이다.

자바스크립트에서 모든 객체는 <span class="variable">[[Prototype]]</span>이라는 내부 슬롯을 갖는다. 그리고 이는 직접 접근이 불가하며 <span class="variable">\_\_proto\_\_</span>틑 통해 간접 접근할 수 있다. <span style="font-size: 14px;">(이전글: <a href="https://www.howdy-mj.me/javascript/prototype-and-proto/" target="_blank" class="link">[[Prototype]], \_\_proto\_\_와 prototype</a>)</span>

---

그나마 'Object.getOwnPropertyDescriptor()'를 통해 간접적으로 확인할 수 있다.

---

자바스크립트 객체의 <a href="https://262.ecma-international.org/11.0/#table-5" target="_blank" class="link">필수 내부 메서드</a>는 <span class="variable">[[GetPrototypeOf]]</span>, <span class="variable">[[SetGPrototypeOf]]</span>, <span class="variable">[[IsExtensible]]</span>, <span class="variable">[[PreventExtensions]]</span>, <span class="variable">[[GetOwnProperty]]</span>, <span class="variable">[[DefineOwnProperty]]</span>, <span class="variable">[[HasProperty]]</span>, <span class="variable">[[Get]]</span>, <span class="variable">[[Set]]</span>, <span class="variable">[[Delete]]</span>, <span class="variable">[[OwnPropertyKeys]]</span>가 있다.

## 함수의 내부 메서드

함수도 객체이므로 일반 객체와 동일하게 동작하여 위에 언급한 일반 객체의 내부 슬롯과 내부 메서드 모두를 갖고 있다.

하지만 함수는 일반 객체와 다르게 **'호출이 가능'**하다. 함수 객체는 함수로서 동작하기 위해 함수 객체만을 위한 <span class="variable">[[Environment]]</span>, <span class="variable">[[FormalParameters]]</span> 등의 내부 슬롯과 <span class="variable">[[Call]]</span>, <span class="variable">[[Construct]]</span> 같은 내부 메서드를 추가로 갖고 있다. <span style="font-size: 14px;">(ECMA-262: <a href="https://262.ecma-international.org/11.0/#table-6" target="_blank" class="link">함수 객체의 내부 메서드</a>)</span>

내부 메서드 <span class="variable">[[Call]]</span>을 갖는 함수 객체는 callable이라 하며, 내부 메서드 <span class="variable">[[Construct]]</span>를 갖는 함수 객체를 constructor, <span class="variable">[[Construct]]</span>를 갖지 않는 함수 객체를 non-constructor라고 부른다.

```js
function func() {}

// 1. [[Call]]를 갖는 callable 함수 객체
func()

// 2. [[Construct]]를 갖는 함수 객체
new func()
```

즉, 함수 객체는 모두 callable하여 호출할 수 있지만, constructor와 non-constructor로 나뉘어 생성자 함수로서 호출 여부는 갈린다.

<br>

### constructor와 non-constructor

함수 객체를 정의하는 방식에 따라 두 개로 나뉜다.

- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: 화살표 함수, 메서드(ES6 메서드 축약 표현)

<br>
<br>
<br>

---

### 함수 객체

[https://262.ecma-international.org/11.0/#function-object](https://262.ecma-international.org/11.0/#function-object)

[[Call]](thisArgument, argumentsList)

얘는 빌트인 함수 객체를 위한 내부 메서드인데, thisArgument와 argumentList(ECMA스크립트 언어 값 리스트)를 받을 수 있음.

[[Construct]](argumentList, newTarget)

얘는 빌트인 함수 객체를 위한 내부 메서드인데, 파라미터로는 argumentList와 newTarget을 받는다.

[[Call]]과 다르게 constructor 객체를 지원한다. 모든 constructor는 함수 객체이다. 그러므로, constructor는 constructor 함수나 constructor 함수 객체를 참조될 수 있다.

'new' operator나 'super'를 부름.

- [https://javascript.info/constructor-new](https://javascript.info/constructor-new)

그런데 class Person {} 도 function으로 나오네.... p424

--

p469

<br />

**참고**

<div style="font-size: 12px;">

- https://stackoverflow.com/questions/42467581/why-does-typeof-function-return-function

</div>
