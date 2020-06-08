---
title: 'emotion.js 소개 및 사용법'
date: 2020-6-9 23:21:13
category: 'CSS'
draft: true
---

## 1. emotion.js란?

emotion.js는 CSS-in-JS의 하나로, CSS를 JavaScript 안에서 작성하게 해준다.

emotion.js에는 두가지 방법이 있는데, 하나는 framework agnostic<span style="font-size: 14px;">(\*쉽게 말하면 프레임워크를 사용하지 않는 것)</span>, 하나는 React로 만든 것이다.

## 2. emotion.js 사용법

```sh
# Framework Agnostic
$ npm install emotion

# React
$ npm install @emotion/core
```

_해당 글은 React를 기준으로 작성한다._

emotion.js를 사용해야 할 컴포넌트에 먼저 import를 해야 한다.

```js{1}
/** @jsx jsx */
import { jsx, css } from '@emotion/core'
```

`/** @jsx jsx */`는 babel에게 `React.createElement` 대신 jsx를 jsx라는 함수로 변환하라는 뜻이다. <span style="font-size: 14px;">(출처: [jsx-pragma](https://emotion.sh/docs/css-prop#jsx-pragma))</span>

단순히 주석이라 생각하고 쓰지 않는다면 `@emotion/core`가 적용되지 않는다.

공식 문서에 있는 예문을 같이 살펴보자.

<iframe
     src="https://codesandbox.io/embed/emotionjs-intro-1ysvo?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="emotion.js intro"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-autoplay allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

```js
/** @jsx jsx */
import { css, jsx } from '@emotion/core'

const divStyle = css`
  background-color: hotpink;
  font-size: 24px;
  border-radius: 4px;
  padding: 32px;
  text-align: center;
  &:hover {
    color: white;
  }
`

export default function App() {
  return <div css={divStyle}>Hover to change color.</div>
}
```

기존에 Styled-Component를 써본 사람이라면 익숙한 구조일 것이다.

다른 점이 있다면, `return`문 안에서 `<div>`를 유지하면서 `css`에서
