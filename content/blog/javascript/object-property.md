---
title: 'object의 property 파헤치기'
date: 2021-5-01 00:00:00
category: 'javascript'
draft: true
---

자바스크립트의 참조타입은 모두 객체이기 때문에 <span class="return">object</span>라 반환되었던 것이었다. 때문에 MDN에서도 이를 참조 타입이 아닌 <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures" target="_blank" class="link">객체(Objects)</a>라 정의하고 있는 것 같다.

그나마 'Object.getOwnPropertyDescriptor()'를 통해 간접적으로 확인할 수 있다. (https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

## 객체 종류

native object, built-in object, host object

<!-- <div style="text-align: center;"><img src="https://i.stack.imgur.com/Kfe6W.png" alt="img" /></div> -->
