---
title: 'Next.js 설치, 라우팅, 구동방식'
date: 2020-7-03 20:10:43
category: 'Next.js'
draft: false
---

본 글은 [공식 문서](https://nextjs.org/docs/getting-started) 기준으로 작성되었습니다.

## 소개

React는 CSR이기에 SEO가 안된다는 치명적인 단점이 있다. 물론 cra eject로 설정하여 적용할 수 있지만, 번거롭기 때문에 더 편리하게 사용할 수 있는 Next.js(이하 Next)가 나왔다.

Next가 나오면서 SSR가 되며, 더 빠르게 페이지를 불러오기 위해 코드 스프릿도 지원한다.

## 설치

```sh
$ npx create-next-app 폴더명
$ npm install next react react-dom
```

정상적으로 설치 되었다면 `package.json`에 아래와 같은 scripts가 있는 걸 확인할 수 있다.

```json
"scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
},
```

`react`와 달리 생성 후 아무것도 존재하지 않는다. 그래서 폴더부터 하나씩 만들어야 한다.
`pages` 폴더에 `index.js`를 만들어보자.

폴더 구조

```
node_modules/
pages
└─ index.js
package.json
packgae-lock.json
```

`index.js`

```js
function HomePage() {
  return <div>Welcome to Next.js!</div>
}

export default HomePage
```

그 후, `npm run dev`로 서버를 돌리면 http://localhost:3000 에서 서버가 열린다.

```sh
$ npm run dev

ready - started server on http://localhost:3000
event - compiled successfully
event - build page: /next/dist/pages/_error
wait  - compiling...
event - compiled successfully
event - build page: /
wait  - compiling...
event - compiled successfully
```

`npm run dev`을 하면 위의 코드가 CLI창에 뜨면서 `.next`라는 폴더가 생기는 것을 볼 수 있다.

<p style="text-align: center;"><img src="./images/01.png">
</p>

이를 통해 next.js는 자동으로 컴파일과 빌드(웹팩과 바벨로)를 진행하며 `/`에 페이지를 정적으로 페이지를 만드는 것을 확인할 수 있다.

## Router

React와 달리 Next.js는 static한 페이지를 만들며, `pages` 폴더 안에 있는 js 파일이 하나의 URL처럼 작동된다.

그렇다면 about 페이지를 만들어보자.

```
node_modules/
pages
├─ index.js
└─ about.js
package.json
packgae-lock.json
```

`pages/about.js`

```js
function About() {
  return <div>About</div>
}

export default About
```

그리고 `pages/index.js`에 Link를 추가해주자.

```js
import Link from 'next/link'

function HomePage() {
  return (
    <div>
      Welcome to Next.js!
      <br />
      <Link href="/about">About</Link>
    </div>
  )
}

export default HomePage
```

<p style="text-align: center"><img src="./images/02.png" ></p>

그러면 위와 같은 화면이 렌더되며, About을 누르면 `localhost:3000/about`에 생성한 `about.js` 화면이 표시된다.

## Next.js의 SSR

여기까지 와서 나는 Next.js가 어떻게 SSR이 가능한지 궁금해졌다.

그러기 위해서는 Next.js의 구동 순서에 대해 알아야한다.

Next.js는 `_app.js`와 `_document.js`가 제일 처음에 실행된다. 두 파일 모두 `pages` 폴더 안에 있어야 한다.

우리가 맨 처음에 프로젝트를 생성할 때 없는 파일이지만, Next 자체에서 제공하는 로직으로 실행된다. 따라서 프로젝트 입맛에 맞게 만들기 위해서는 커스터마이징을 해야 하는데 바로 이 두개의 파일에서 진행된다.

두 파일 모두 Server only file로 클라이언트 단에서 사용하는 함수(ex. addEventlistner, window 등)를 사용하면 안된다.

<h3>_app.js</h3>

최초로 실행되는 파일로, Client에서 띄워지는 전체 컴포넌트의 레이아웃이라 이해하면 된다. 공통 레이아웃으로 최초에 실행되어 내부에 들어갈 컴포넌트들을 실행한다.

```js
import React from 'react'
import App from 'next/app'

function App {
  render() {
    const { Component, ...other } = this.props
    return <Component {...other} />
  }
}

export default App
```

여기서 `Component`란 props로 받은 페이지들을 뜻한다.

<h3>_document.js</h3>

그 다음에 `_document.js`가 실행되는데, 이는 `_app.js`에서 구성한 HTML이 어떤 형태로 들어갈지 구성해주는 것이다.

```js
import Document, { Html, Head, Main, NextScript } from 'next/document'

class MyDocument extends Document {
  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    )
  }
}

export default MyDocument
```

<h3>getInitialProps</h3>

React에서 프로젝트를 진행하면 렌더링 후에 `componentDidMount`나 `useEffect()`로 데이터를 불러와야 한다. 하지만 Next에서는 `getInitialProps`를 통해 데이터를 미리 불러와 한 번에 렌더링이 가능하다. 미리 데이터를 불러옴으로 속도가 빨라지며, 코드 상의 처리가 깔끔해진다.

_Next 9.3 이후로는 `getStaticProps`나 `getServerSideProps` 사용을 권장한다. 추후 [추가](https://nextjs.org/docs/basic-features/data-fetching) 예정_

만약 어디에서나 공통된 데이터가 필요하다면 `_app.js`에 `getInitialProps`를 붙이면 되고, 각기 다른 데이터가 필요하다면 페이지마다 `getInitialProps`를 붙이면 된다.

각 페이지마다 `getInitialProps`를 붙이는 방법은 아래와 같다.

```js
function Page({ stars }) {
  return <div>Next stars: {stars}</div>
}

Page.getInitialProps = async ctx => {
  const res = await fetch('https://api.github.com/repos/vercel/next.js')
  const json = await res.json()
  return { stars: json.stargazers_count }
}

export default Page
```

**주의**:

- `getInitialProps`으로 리턴되는 객체는 Date, Map, Set으로 사용되는 것이 아닌 순수 객체여야 한다.
- `getInitialProps`은 자식 컴포넌트에서 사용할 수 없으며, 오로지 default export 컴포넌트에서만 사용할 수 있다.

<h4>Context Object</h4>

`getInitialProps`는 `context`라는 단일 인자를 받는데, 설정하지 않는다면 기본값으로 설정된다.

- `pathname` - `pages` 폴더 안에 있는 현재 Route
- `query` - 객체로 이루어진 쿼리스트링, ex. /category?id=phone에서 {id: 'phone'}
- `asPath` - query를 포함한 `String`의 실제 경로, ex. /category?id=phone 전체 경로
- `req` - HTTP request object (server only)
- `res` - HTTP response object (server only)
- `err` - Error object if any error is encountered during the rendering

<br>

**참고**

<div style="font-size: 12px;">

- https://nextjs.org/
- https://velog.io/@cyranocoding/Next-js-%EA%B5%AC%EB%8F%99%EB%B0%A9%EC%8B%9D-%EA%B3%BC-getInitialProps

</div>
