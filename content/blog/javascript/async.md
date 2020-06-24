---
title: '자바스크립트의 비동기 함수 알아보기'
date: 2020-5-21 23:21:13
category: 'JavaScript'
draft: true
---

## 비동기(Asynchronous) 함수란?

자바스크립트는 [싱글스레드](https://howdy-mj.netlify.app/node/about-node.js/#%EC%8A%A4%EB%A0%88%EB%93%9Cthread)이기 때문에 한 번에 하나의 작업만 수행할 수 있다. 이를 해결하기 위해 **비동기**가 생겼다.

비동기란 특정 코드의 처리가 끝나기 전에 다음 코드를 실행할 수 있는 것을 뜻한다.

카페에 비유하자면, 만약 알바생이 한 명이라면 주문과 음료 제조를 동시에 하지 못한다. 하지만 여러 명이 있다면, 한명이 주문을 받으면 그 순서에 맞게 다른 사람이 음료 제조를 하여 주문과 음료 제조를 동시에 할 수 있다.

자바스크립트는 즉시 처리하지 못하는 이벤트들을 이벤트 루프에 모아 놓고, 먼저 처리해야하는 이벤트를 실행한다.

## 비동기 방식

자바스크립트에는 콜백 함수, Promise, async await 이렇게 크게 3가지 비동기 방식이 존재한다.

### 콜백(Callback) 함수

자바스크립트 비동기하면 '콜백 지옥'이라는 단어를 몇 번 봤을 것이다.

콜백 함수는 하나만 썼을 때는 간단하지만, 비동기로 함수의 매개 변수에 다른 콜백 함수가 중첩되어 사용된다면 그 코드를 보기에 굉장히 어렵고 유지보수도 힘들어진다.

```js
step1(function(value1) {
  step2(function(value2) {
    step3(function(value3) {
      step4(function(value4) {
        step5(function(value5) {
          step6(function(value6) {
            // Do something with value6
          })
        })
      })
    })
  })
})
```

이 코드에서 에러를 잡기 위해서는 각 콜백 함수마다 에러 처리를 써야하는데 그럼 코드량도 어마어마해진다.

```js
step1(function(err, value1) {
  if (err) {
    console.log(err)
    return
  }
  step2(function(err, value2) {
    if (err) {
      console.log(err)
      return
    }
    step3(function(err, value3) {
      if (err) {
        console.log(err)
        return
      }
      step4(function(err, value4) {
        // ,...
      })
    })
  })
})
```

### Promise

위의 콜백 문제를 해결하기 위해 ECMAScript 2015에 Promise가 도입되었다.

Promise는 latency, delay(지연) 때문에 현재 당장 얻을 수 없지만 가까운 미래에 얻을 수 있는 데이터에 접근하기 위한 방법을 제공한다.

### async await

<br />

**참고**

<div style="font-size: 12px;">

- https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/
- https://joshua1988.github.io/web-development/javascript/js-async-await/
- https://velog.io/@yejinh/%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0
- https://librewiki.net/wiki/%EC%BD%9C%EB%B0%B1_%EC%A7%80%EC%98%A5
- https://www.daleseo.com/js-async-callback/

</div>
