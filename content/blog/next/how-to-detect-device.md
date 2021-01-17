---
title: 'Next.js에서 userAgent 정보 가져오기'
date: 2021-1-19 02:57:43
category: 'next'
draft: false
---

Next.js에서 라이브러리 설치 없이 유저가 사용하는 기기를 탐지하는 방법에 대해 알아보자.

해당 글은 next `^10.0.3`, react `^17.0.1`, typescript `^4.1.3` 버전으로 작성되었다.

## userAgent 정보 가져오기

만들어진 Next 프로젝트의 `pages/index.tsx`에서 아래와 같이 작성하면 된다.

```tsx
// 'pages/index.tsx'에서 getInitialProps 사용
import { NextPage, NextPageContext } from 'next'
import Head from 'next/head'

interface Props {
  userAgent?: string
}

const Home: NextPage<Props> = ({ userAgent }) => {
  console.log('index', userAgent)
  return (
    <div>
      <Head>
        <title>howdy-mj</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <div>Hello World!</div>
    </div>
  )
}

Home.getInitialProps = async ({ req }: NextPageContext) => {
  const userAgent = req ? req.headers['user-agent'] : navigator.userAgent
  return { userAgent }
}

export default Home
```

그럼 console에서 아래와 같이 정상적으로 필자가 사용하는 기기의 정보를 가져올 수 있다.

```
Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36
```

<br />

하지만 이렇게 사용하기에 찝찝한 점이 두 가지 있었다.

1. 필자는 어느 페이지를 띄우든 특정 브라우저(ex. IE)를 통해 들어오는 유저에게 모달창을 띄우고 싶었다.

2. Next 9.3 이상에서 `getInitialProps` 대신 `getStaticProps` 사용을 권장하고 있지만, 정작 `getStaticProps`으로는 위와 같은 결과를 얻을 수 없었다.

```tsx{25-28}
// 'pages/index.tsx'에서 getStaticProps를 사용하면 에러 발생
import { NextPage, NextPageContext } from 'next'
import Head from 'next/head'

interface Props {
  userAgent?: string
}

const Home: NextPage<Props> = ({ userAgent }) => {
  console.log('index', userAgent)

  return (
    <div>
      <Head>
        <title>howdy-mj</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <div>Hello World!</div>
    </div>
  )
}

export const getStaticProps = ({ req }: NextPageContext) => {
  const userAgent =
    typeof navigator === 'undefined'
      ? req.headers['user-agent']
      : navigator.userAgent

  return { props: { userAgent } }
}
```

처음에 navigator가 undefined되어 `typeof navigator === 'undefined'` 조건을 넣었지만, 여전히 `TypeError: Cannot read property 'headers' of undefined`라는 타입 에러를 뱉으며 결과를 얻을 수 없었다. 아직 정확한 이유를 찾지 못했다.

## 모든 페이지에서 userAgent 알아내기

`pages/_app.tsx`에서 제일 처음에 `getInitialProps`를 사용한 코드를 넣으면 `ReferenceError: navigator is not defined`와 같은 에러가 나왔다.

하지만 정말 희한하게도 `getStaticProps`를 사용하면 참조에러가 나지 않는다. 둘다 SSR이기에 같은 에러가 나야 하지만 나지 않았다. 대신 console에 userAgent가 undefined로 뜬다. (이 역시 아직 이유를 찾지 못했다..)

원래는 `getStaticProps`에서 특정 브라우저인지 알아내어 바로 props로 넘겨주고 싶었으나, undefined 이기 때문에 넘겨주지 못하고 useEffect를 사용할 수 밖에 없었다.

```tsx{19, 28}
// 'pages/_app.tsx`에서 getStaticProps 사용
import { useEffect, useState } from 'react';
import type { AppProps } from 'next/app';
import { NextPageContext } from 'next';
import { ThemeProvider } from 'styled-components';

import GlobalStyle from '../styles/reset';
import theme from '../styles/theme';

interface userAgentProps {
  userAgent: string;
}

function MyApp(
  { Component, pageProps }: AppProps,
  { userAgent }: userAgentProps,
) {
  const [currentBrowser, setCurrentBrowser] = useState<boolean>(false);
  console.log('_app', userAgent); // output: undefined

  useEffect(() => {
    const matchIE = navigator.userAgent.match(/MSIE|rv:|IEMobile/i);
    if (matchIE) {
      setCurrentBrowser(Boolean(matchIE));
    }
  }, []);

  console.log(currentBrowser); // output: false

  return (
    <ThemeProvider theme={theme}>
      <GlobalStyle />
      <Component {...pageProps} />
    </ThemeProvider>
  );
}

export const getStaticProps = ({ req }: NextPageContext) => {
  const userAgent =
    typeof window === 'undefined'
      ? req.headers['user-agent']
      : window.navigator.userAgent;

  return { props: { userAgent } };
};

export default MyApp;

```

하지만 `currentBrowser` 값은 boolean으로 뜨기 때문에 필자가 원하던 모달창은 띄울 수 있었다.

## 정리

특정 페이지에서의 사용 기기를 알고 싶다면, 해당 페이지 내에서 `getInitialProps`를 사용하면 된다.

전체 페이지에서 사용 기기를 알고 싶다면, `pages/_app.tsx`에서 `getStaticProps`와 `useEffect`에서 `match`로 특정 브라우저인지 아닌지를 Boolean 값으로 찾아내면 된다.

```tsx
useEffect(() => {
  const matchIE = navigator.userAgent.match(/MSIE|rv:|IEMobile/i)
  const Chrome = navigator.userAgent.match(/Chrome/i)
  if (matchIE) {
    setCurrentBrowser(Boolean(matchIE))
  }
  if (Chrome) {
    console.log(Boolean(Chrome))
  }
}, [])
```

<br />

### 궁금점

getInitialProps와 getStaticProps의 차이, 그리고 넘긴 props가 왜 undefined인지 궁금하다.

<br />

**참고**

<div style="font-size: 12px;">

- https://nextjs.org/docs/api-reference/data-fetching/getInitialProps

</div>
