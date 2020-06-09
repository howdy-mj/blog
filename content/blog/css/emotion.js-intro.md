---
title: 'emotion.js 소개 및 사용법 (feat. CSS-in-JS)'
date: 2020-6-9 12:04:13
category: 'CSS'
draft: false
---

## 1. emotion.js란?

emotion.js는 CSS-in-JS의 하나로, CSS를 JavaScript 안에서 작성하게 해준다.

### CSS-in-JS란?

CSS-in-JS(JSS)는 JavaScript를 사용하여 선언적이고, 분쟁 없이 재사용 가능한 방식으로 스타일을 해주는 CSS이다. JS를 CSS로 전환하는 고성능 컴파일러로, 브라우저, 런타임, 서버 사이드에서 작동한다.

<br>

다시 돌아와서, emotion.js는 크게 두가지 사용 방법이 있는데, 하나는 framework agnostic<span style="font-size: 14px;">(\*쉽게 말하면 프레임워크를 사용하지 않는 것)</span>, 하나는 React를 이용한 방법이다.

## 2. emotion.js 사용법

### 설치

```sh
# Framework Agnostic
$ npm install emotion

# React
$ npm install @emotion/core
```

_해당 글은 React, @emotion/core 10.0.28 기준으로 작성되었다._

emotion.js를 사용해야 할 컴포넌트에 먼저 import를 해야 한다.

```js{1}
/** @jsx jsx */
import { jsx, css } from '@emotion/core'
```

`/** @jsx jsx */`는 babel에게 `React.createElement` 대신 jsx를 jsx라는 함수로 변환하라는 뜻이다. <span style="font-size: 14px;">(출처: [jsx-pragma](https://emotion.sh/docs/css-prop#jsx-pragma))</span>

단순히 주석이라 생각하고 쓰지 않는다면 `@emotion/core`가 적용되지 않는다.

### 기본 구조

공식 문서에 있는 예문을 같이 살펴보자.

<iframe
     src="https://codesandbox.io/embed/emotionjs-intro-1ysvo?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="emotion.js intro"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-autoplay allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

(기존 예문에서 구조만 살짝 수정했다)

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

```js
// Styled-Component
import styled from 'styled-components'

const DivStyle = styled.div`
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
  return <DivStyle>Hover to change color.</DivStyle>
}
```

개인적으로 `emotion.js`가 더 편하다고 느낀 점은, jsx안에서 이게 어떤 태그인지 바로 알 수 있다는 점이다.

`Styled-Component`는 component 이름에 `div`, `p`등을 써놓지 않으면 이게 어떤 태그인지 바로 알 수 없다. 만약 해당 js파일에 내용이 많아 일일히 찾아야 한다면 엄청 번거롭다.

### 라벨링

`emotion.js`는 `Styled-Component`처럼 브라우저에서 열었을 때 className을 임의로 생성해준다.

<p style="text-align: center;"><img src="./images/emotion/01.PNG" /></p>

`emotion.js`는 [babel-plugin-emotion](https://emotion.sh/docs/babel-plugin-emotion)을 사용하면 커스터마이징이 가능하다.

```sh
$ npm install --save-dev babel-plugin-emotion
# or
$ yarn add --dev babel-plugin-emotion
```

```js{10}
const divStyle = css`
  background-color: hotpink;
  font-size: 24px;
  border-radius: 4px;
  padding: 32px;
  text-align: center;
  &:hover {
    color: white;
  }
  label: divStyle;
`
```

```html
<div class="css-mfy11-divStyle">
  ...
</div>
```

이렇게 바로 `css`에 label을 넣어 변경하는 방법도 있지만, 이는 매번 label 값을 넣어줘야 하고, 다른 개발자들과 같이 일하기에도 불편하기 때문에 `.babelrc`를 만들어 저장하는 것을 권장한다.

`.babelrc`

```json
{
  "plugins": [
    [
      "emotion",
      {
        "autoLabel": true,
        "labelFormat": "[dirname]-[filename]-[local]"
      }
    ]
  ]
}
```

_아직 .babelrc의 정확한 적용 방법을 모르겠다... 확인해보고 추후 추가 예정_

이 방법 외, 최근 CRA에서는 [Babel Macros](https://emotion.sh/docs/babel-macros)를 지원하고 있다. 이는 Babel config없이 babel를 전환할 수 있는 것인데, `Styled-Component`도 이를 통해 className 라벨을 수정할 수 있다.

하지만 아직 Babel Macros는 몇 최적화 작업이 불가능하기 때문에 `babel-plugin-emotion` 사용을 권장하고 있다.

### 재사용

CSS-in-JS의 핵심이 바로 '재사용성'이다.

emotion.js를 재사용하는 것도 간단하다. component처럼 만들어서 바로 가져다 쓰면 된다.

<iframe
     src="https://codesandbox.io/embed/great-pare-czppt?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:200px; border:0; border-radius: 4px; overflow:hidden;"
     title="emotion component"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-autoplay allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

[출처](https://emotion.sh/docs/css-prop)

```js
/** @jsx jsx */
import { jsx } from '@emotion/core'

const P = props => (
  <p
    css={{
      margin: 0,
      fontSize: 12,
      lineHeight: '1.5',
      fontFamily: 'sans-serif',
      color: 'blue',
    }}
    {...props}
  />
)

const ArticleText = props => (
  <P
    css={{
      fontSize: 20,
      fontFamily: 'Georgia, serif',
      color: 'darkgray',
    }}
    {...props}
  />
)

export default function App() {
  return (
    <div>
      <P>Using P component</P>
      <ArticleText>Using ArticleText component</ArticleText>
    </div>
  )
}
```

같은 CSS 속성이 있다면, 제일 최근 것으로 덮어씌워진다. (ex. `ArticleText`의 fontSize, fontFamily, color로 대체)

만약 component로 사용하여, CSS 속성을 inline으로 쓴다면 따로 `css`를 써줄 필요도 없으며, 속성명을 `camelCase`로 작성해야 한다. (ex. fontSize, fontFamily, backgroundColor 등)

### styled

emotion.js에서도 `styled`를 가져올 수 있다.

이는 `Styled-components`와 `Glamorous`에서 영향을 많이 받아 작성되었다. 사용법은 `Styled-components`와 거의 비슷하다.

Styled-components는 다른 글에서 더 자세히 써보겠다.

### Nested

emotion.js에서 Nested도 사용가능하다.

```js
/** @jsx jsx */
import { jsx, css } from '@emotion/core'

const paragraph = css`
  color: turquoise;

  a {
    border-bottom: 1px solid red;
    cursor: pointer;
  }
`
render(
  <p css={paragraph}>
    Some text.
    <a>A link with a bottom border.</a>
  </p>
)
```

### 미디어 쿼리

반응형을 구축해야 하는 미디어 쿼리는 일반 CSS(SCSS)에서 하는 것과 비슷하게 작성하면 된다.

```js
/** @jsx jsx */
import { jsx, css } from '@emotion/core'

render(
  <p
    css={css`
      font-size: 30px;
      @media (min-width: 420px) {
        font-size: 50px;
      }
    `}
  >
    Some text!
  </p>
)
```

이 외에도, 미리 breakpoint를 선언하여 재사용 가능하게 만드는 법과 `facepaint` 패키지를 설치하여 더 쉽게 breakpoints를 만들 수도 있다.

[emotion - Media Queries](https://emotion.sh/docs/media-queries)

<br>

## 번외) 왜 CSS-in-JS를 사용할까?

위에 작성된 내용과 종합하여 정리하면 아래와 같다.

- component로 만들어 재사용
- 중복되는 className 해결 (Global namespace)
- 자바스크립트에서 쓰이는 상수, props, 함수 공유하기
- 상속에 의한 영향이 없도록 격리 (Isolation)
- 미사용 코드 처리 (Dead Code Elimination)

어떤 글을 보면 CSS-in-JS로 '디자이너와 협업을 더 원활하게 할 수 있다'고 되어있다. 하지만 개인적으로 CSS-in-JS는 단순히 개발자들이 더 편하게 쓰기 위해 생긴 것이지, 디자이너와 협업을 위해 만들어진 것이 아닌 것 같다. 디자이너에게 가서 CSS-in-JS를 보여주면 어떻게 사용하는지 모르는 사람이 대부분일 것이라 생각한다.

또한, className을 짓지 않아도 된다는 장점이 있다고 하지만, 결국 사용해야 할 이름을 지어야 하는 건 똑같다. 하지만 scope가 있어서 다른 곳에서 중복으로 이름을 사용가능한 장점은 있다.

<br>

**참고**

<div style="font-size: 12px;">

- https://cssinjs.org/?v=v10.2.0
- https://d0gf00t.tistory.com/22
- https://medium.com/@okys2010/%EB%AA%A8%EB%8D%98-css-1-css-in-js-c1c53d9bbbc9
- https://medium.com/@oleg008/jss-is-css-d7d41400b635

- https://orezytivarg.github.io/css-evolution-from-css-sass-bem-css-modules-to-styled-components/

- https://ideveloper2.dev/blog/2019-05-05--thinking-about-emotion-js-vs-styled-component/

<div>
