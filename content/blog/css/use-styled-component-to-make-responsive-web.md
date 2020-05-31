---
title: 'Styled Component로 반응형 구축하기'
date: 2020-5-21 23:21:13
category: 'CSS'
draft: true
---

## 1. Styled Component란?

Styled Component는 CSS-in-JS의 하나로, CSS를 하나의 컴포넌트로 만들어 주는 것이다.

**Styled Component 설치**

```sh
npm install --save styled-components
```

<iframe
     src="https://codesandbox.io/embed/styled-components-button-f89hh?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:300px; border:0; border-radius: 4px; overflow:hidden;"
     title="Styled-Components Button"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-autoplay allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

Button 자체가 하나의 Component가 되어 props도 바로 넘겨줄 수 있다.

더 자세한 내용은 공식 [홈페이지](https://styled-components.com/)를 참고하길 바란다.

<br>

Styled Component를 이용하면
반응형을 만드려면 css 속성에 매번 `@media (min-width: 770px) {...}`을 써줘야 한다.
컴포넌트 수가 몇 없고, 안에 적용해야 할 div도 몇 개 없다면 하나 씩 쳐도 괜찮다.
하지만 적용해야 할 컴포넌트 수가 엄청 많다면, 코드를 작성하다가 오타가 날 수도 있고, 숫자를 잘 못 적었을 수도 있기 때문에 원인을 찾는데 시간이 오래 걸릴 것이다.

따라서, 전역에서 사용 할 수 있는 `theme.js`를 만들어 주는 것이 좋다.

## 2. theme.js 만들기
