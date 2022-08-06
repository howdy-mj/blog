---
title: 'Next.js의 렌더링 과정(Hydrate) 알아보기'
date: 2022-8-6 00:00:00
category: 'next'
draft: false
---

누군가 나에게 Next.js를 쓰는 이유를 물어본다면, 가장 먼저 SSR 때문이라고 대답할 것 같다. Next.js 공식 홈페이지에서도 가장 먼저 강조하고 있는 것이 'hybrid static & server rendering'인 것처럼 말이다.

하지만 정확히 어떠한 과정을 거쳐 렌더링이 되는지 몰라서 찾아보았다.

<div style="margin-bottom: 30px"></div>

### Next.js의 Pre-rendering

React는 CSR(Client-side Rendering)로, 처음에 브라우저가 빈 HTML을 파일을 받아 아무것도 보여주지 않다가, 사용자의 기기에서 렌더링이 진행되어 한 번에 화면을 보여준다.

<div class="img-div">
  <img src="https://nextjs.org/static/images/learn/foundations/client-side-rendering.png" alt="CSR">
  <p>https://nextjs.org/learn/foundations/how-nextjs-works/rendering</p>
</div>

반면, Next.js는 모든 페이지를 미리 렌더링(pre-render)한다. 이는 Next.js가 모든 일을 클라이언트 측에서 모든 작업을 수행하는 것이 아니라, 각 페이지의 HTML을 미리 생성하는 것이다. 생성된 HTML은 해당 페이지에 필요한 최소한의 자바스크립트 코드와 연결된다. 그 후 브라우저에 의해 페이지가 로드되면, 자바스크립트 코드가 실행되어 페이지와 유저가 상호작용할 수 있게 된다.

이러한 과정을 hydration이라 하며, 곧 관련 코드를 훑어볼 예정이다.

<div class="img-div">
  <img src="https://nextjs.org/static/images/learn/foundations/pre-rendering.png" alt="SSR">
  <p>https://nextjs.org/learn/foundations/how-nextjs-works/rendering</p>
</div>

<br>

그리고 Next.js에서 미리 렌더링 하는 방식은 두 가지로 나뉘며, HTML이 생성되는 시점이 다르다. 하나는 빌드 타임에 HTML에 생성되어 매 요청마다 이를 재사용하게 해주는 SSG(Static-site Generation)이고, 다른 하나는 매 요청마다 HTML을 생성하는 SSR(Server-side Rendering)이다.

Next.js는 기본적으로 SSG를 이용해 정적인 페이지를 미리 생성하여 SEO에 유리하다. 따라서, 블로그, 포트폴리오, 메뉴얼 등 데이터가 바뀌지 않는 페이지는 SSG를 사용한다. 반면, 유저의 요청마다 데이터가 변경될 수 있는 맞춤 추천리스트, 장바구니 페이지 등은 SSR을 사용해야 한다.

## Next.js 렌더링 순서

> 아래 코드 모두 <a href="https://github.com/vercel/next.js/tree/canary/packages/next" target="_blank">next.js/packages/next</a>를 base로 잡고 경로를 작성했으며, 해당 글에서 필요한 부분만 가져왔다. 우측의 Code 링크를 통해 소스코드를 볼 수 있다.

Next.js가 먼저 Server를 거친 후에 Client가 렌더링 되는건 알겠다. 그런데 어떤 코드들을 거쳐서 실제 브라우저에서 볼 수 있는 걸까?

<br>

흐름대로 Next.js의 Server의 <a href="" target="_blank">render.tsx</a>부터 살펴보았다.

<span class="file-location">server/render.tsx</span> <a class="small" href="https://github.com/vercel/next.js/blob/canary/packages/next/server/render.tsx#L310" target="_blank">(Code)</a>

```tsx{23}
export async function renderToHTML(
  req: IncomingMessage,
  res: ServerResponse,
  pathname: string,
  query: NextParsedUrlQuery,
  renderOpts: RenderOpts
): Promise<RenderResult | null> {
  // ...

  const renderDocument = async () => {
    // ...
    async function loadDocumentInitialProps(
      renderShell?: (
        _App: AppType,
        _Component: NextComponentType
      ) => Promise<ReactReadableStream>
    ) {
      // ...
      const renderPage: RenderPage = (
        options: ComponentsEnhancer = {}
      ): RenderPageResult | Promise<RenderPageResult> => {
        // ...
        const html = ReactDOMServer.renderToString(
          <Body>
            <AppContainerWithIsomorphicFiberStructure>
              {renderPageTree(EnhancedApp, EnhancedComponent, {
                ...props,
                router,
              })}
            </AppContainerWithIsomorphicFiberStructure>
          </Body>
        )
        return { html, head }
      }
    }
    // ...
    return {
      bodyResult,
      documentElement,
      head,
      headTags: [],
      styles,
    }
  }
}
```

코드의 양이 상당히 많지만, 현재 중점적으로 봐야하는 부분은 서버에서 어떻게 HTML을 렌더링하고 있는가이다. 그 결과, `renderPage`에서 HTML을 만드는 코드를 찾을 수 있었고, 이 코드는 `ReactDOMServer.renderToString()`을 이용해 ReactNode를 HTML 문자열로 만들고 있다.

### renderToString() <a class="small" href="https://github.com/facebook/react/blob/main/packages/react-dom/src/server/ReactDOMLegacyServerNode.js#L22" target="_blank">(Code)</a>

```js
ReactDOMServer.renderToString(element)
```

React 엘리먼트의 초기 HTML을 문자열로 반환한다.

<br>

그리고 이렇게 생성된 HTML은 <a href="https://github.com/vercel/next.js/blob/canary/packages/next/server/render.tsx#L1361" target="_blank">htmlProps</a>가 되어 document로 반환된다.

<span class="file-location">server/render.tsx</span> <a class="small" href="https://github.com/vercel/next.js/blob/canary/packages/next/server/render.tsx#L1432" target="_blank">(Code)</a>

```tsx{12,21}
export async function renderToHTML(
  req: IncomingMessage,
  res: ServerResponse,
  pathname: string,
  query: NextParsedUrlQuery,
  renderOpts: RenderOpts
): Promise<RenderResult | null> {
  // ...

  const documentResult = await renderDocument()

  const htmlProps: HtmlProps = {
    __NEXT_DATA__: {
      // ...
    },
  }

  const document = (
    <AmpStateContext.Provider value={ampState}>
      <HtmlContext.Provider value={htmlProps}>
        {documentResult.documentElement(htmlProps)}
      </HtmlContext.Provider>
    </AmpStateContext.Provider>
  )

  const documentHTML = ReactDOMServer.renderToStaticMarkup(document)

  // ...
  // 운영환경 여부에 따라 prefix에 속성을 다르게 하고,
  // prefix와 suffix 정보를 가진 streams를 선언한다
  // 이때 '<!-- __NEXT_DATA__ -->'가 prefix에 입력된다

  if (generateStaticHTML) {
    // ...
    return new RenderResult(optimizedHtml)
  }

  return new RenderResult(
    chainStreams(streams).pipeThrough(
      createBufferedTransformStream(postOptimize)
    )
  )
}
```

Next.js로 만들어진 페이지의 Network 탭에서 서버에서 반환한 HTML을 볼 수 있다.

<div class="img-div">
  <img src="./images/hydrate/nextjs-html.png">
  <p>Next.js 서버에서 반환한 HTML</p>
</div>

<br>

Next.js 서버에서 어떻게 HTML을 생성하고 정보를 입력하는지 알았으니, 이제 <a href="https://github.com/vercel/next.js/blob/canary/packages/next/client/index.tsx" target="_blank">Client</a> 코드를 살펴보자.

<span class="file-location">client/next.js</span> <a class="small" href="https://github.com/vercel/next.js/blob/canary/packages/next/client/next.js#L12:L14" target="_blank">(Code)</a>

```js
initialize({})
  .then(() => hydrate())
  .catch(console.error)
```

우선 `initialize()`가 진행된 다음에 `hydrate()`를 실행하는 것을 알았다. 각각의 코드를 살펴보자.

<span class="file-location">client/index.tsx</span> <a class="small" href="https://github.com/vercel/next.js/blob/canary/packages/next/client/index.tsx#L189" target="_blank">(Code)</a>

```tsx{7}
export async function initialize(
  opts: { webpackHMR?: any } = {}
): Promise<{
  assetPrefix: string
}> {
  initialData = JSON.parse(
    document.getElementById('__NEXT_DATA__')!.textContent!
  )
  window.__NEXT_DATA__ = initialData

  const prefix: string = initialData.assetPrefix || ''

  appElement = document.getElementById('__next')
  return { assetPrefix: prefix }
}
```

`initialize()`는 서버에서 렌더링한 HTML에서 `__NEXT_DATA__` 를 id로 갖는 엘리먼트의 컨텐츠를 브라우저의 전역객체 `window.__NEXT_DATA__`로 저장한다. 그리고 운영 환경에 따라 assetPrefix를 반환한다.

<span class="file-location">client/index.tsx</span> <a class="small" href="https://github.com/vercel/next.js/blob/canary/packages/next/client/index.tsx#L291" target="_blank">(Code)</a>

```tsx
export async function hydrate(opts?: { beforeRender?: () => Promise<void> }) {
  // ...
  const renderCtx: RenderRouteInfo = {
    App: CachedApp,
    initial: true,
    Component: CachedComponent,
    props: initialData.props,
    err: initialErr,
  }

  render(renderCtx)
}
```

`hydrate()`는 실행하려는 페이지의 에러가 있는지 확인 및 validation 체크를 하고 없다면 렌더링할 때 필요한 컨텍스트(ex. 라우터, App, Component, initialProps 등)를 `render()`의 인자로 넘겨준다.

```tsx{8}
async function render(renderingProps: RenderRouteInfo): Promise<void> {
  // ...
  await doRender(renderingProps)
}

function doRender(input: RenderRouteInfo): Promise<any> {
  // ...
  renderReactElement(appElement!, callback => (
    <Root callbacks={[callback, onRootCommit]}>
      {process.env.__NEXT_STRICT_MODE ? (
        <React.StrictMode>{elem}</React.StrictMode>
      ) : (
        elem
      )}
    </Root>
  ))
}
```

`doRender()` 함수를 따라가다보면, `renderReactElement()`를 실행한다

<span class="file-location">client/index.tsx</span> <a class="small" href="https://github.com/vercel/next.js/blob/canary/packages/next/client/index.tsx#L543" target="_blank">(Code)</a>

```tsx{12,15}
let shouldHydrate: boolean = true // 첫 렌더에서는 항상 true이다

function renderReactElement(
  domEl: HTMLElement,
  fn: (cb: () => void) => JSX.Element
): void {
  //...
  const reactEl = fn(shouldHydrate ? markHydrateComplete : markRenderComplete)

  // ...
  if (shouldHydrate) {
    ReactDOM.hydrate(reactEl, domEl)
    shouldHydrate = false
  } else {
    ReactDOM.render(reactEl, domEl)
  }
}
```

그리고 드디어 React에 렌더해주는 `ReactDOM.render()`과 `ReactDom.hydrate()`가 나왔다.

### render() <a class="small" href="https://github.com/facebook/react/blob/main/packages/react-dom/src/client/ReactDOMLegacy.js#L313" target="_blank">(Code)</a>

```js
ReactDOM.render(element, container[, callback])
```

React 엘리먼트를 DOM(container)에 렌더링하고 컴포넌트에 대한 참조를 반환한다. 만약 이미 container 내부에 렌더링 되었다면, 수정이 필요한 DOM만 업데이트한다.

<span class="file-location">React 17의 index.js</span>

```jsx{3}
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

이해가 잘 안갈 수 있으니 코드로 알아보자. React 18에서는 createRoot가 한 번 더 감싸주어서 17 버전의 코드를 가져왔다.

여기서 container는 `index.html` 파일의 root id를 가지고 있는 엘리먼트다. React는 보여주고자 하는 컨텐츠를 HTML의 root 엘리먼트 내부로 넣어주어 페이지를 렌더링해준다. 해당 과정 덕분에 사용자가 웹 페이지에서 상품을 클릭하여 상세 페이지로 이동하는 등의 상호작용이 가능하다.

### hydrate() <a class="small" href="https://github.com/facebook/react/blob/main/packages/react-dom/src/client/ReactDOMLegacy.js#L273" target="_blank">(Code)</a>

```js
ReactDOM.hydrate(element, container[, callback])
```

기본적으로 `render()`와 동일하지만, ReactDOMServer로 렌더링된 HTML에 이벤트 리스너(자바스크립트 코드)를 연결해주기 위해 사용된다.

> React 18부터는 hydrate()가 아니라 <a href="https://github.com/facebook/react/blob/main/packages/react-dom/src/client/ReactDOM.js#L165" target="_blank">hydrateRoot()</a>를 사용하라고 되어있다. 현재 Next.js는 React 17, 18 버전 사용중이라 아직 바꾸지 않은 것 같은데, 18로 마이그레이션 모두 끝나면 바뀔 것 같다.

## Hydration

위의 과정들을 정리해보자.

Next.js는 서버에서 HTML을 문자열로 가져온 후에, 클라이언트에서 서버에서 보내준 HTML을 `hydrate()` 혹은 `render()`하여 브라우저에 렌더링된다. 이 일련의 과정을 <span class="definition">Hydration</span>이라 한다.

<details style="margin-bottom: 20px">
  <summary>Hydration 번역</summary>
  <ul>
    <li>사실 아직 hydration의 마땅한 번역 값을 찾지 못했다.</li>
    <li>Hydrate는 '수화(水化) 시키다'는 뜻이며, 수화(水化)는 어떤 물질이 물과 결합하여 수화물(물을 포함하는 화합물)이 되는 현상이다.</li>
    <li>서버의 데이터가 클라이언트의 DOM과 결합하는 과정을 빗대어 hydrate라는 단어로 정의된 것 같다.</li>
  </ul>
</details>

React는 클라이언트 렌더링만 있어서 유저에게 보여줄 HTML, CSS 그리고 자바스크립트 모두 `render()` 함수를 이용해 생성하여, React가 어떤 DOM을 렌더하는지 알려준다.

반면, Next.js는 서버에서 보여줄 HTML 컨텐츠를 가져오기 때문에 재차 `render()` 함수로 HTML을 생성하여 DOM을 그리는 일은 비효율적이다. 따라서 `hydrate()` 함수로 서버에서 받아온 HTML에 유저가 상호작용할 수 있는 이벤트 리스너만 연결하는 것이다.

### 코드로 확인해보기

Next.js로 만든 프로젝트에서 `getServerSideProps()`를 이용해 데이터를 주입해보자.

```tsx
import React, { useState } from 'react'
import { GetServerSideProps } from 'next'
import styled from 'styled-components'

const StyledContainer = styled.div`
  border: 1px solid blue;
`

type TempProps = {
  name: string
}

const Temp = ({ name }: TempProps) => {
  const [color, setColor] = useState('blue')
  return (
    <StyledContainer onClick={() => setColor('purple')}>
      Temp page: <span style={{ color: color }}>{name}</span>
    </StyledContainer>
  )
}

export default Temp

export const getServerSideProps: GetServerSideProps = async context => {
  return {
    props: {
      name: 'howdy-mj',
    },
  }
}
```

위의 코드를 Network 탭의 HTML을 보면 아래와 같다.

<div class="img-div">
  <img src="./images/hydrate/temp-ssr.png">
  <p>서버에서 반환한 Temp 컴포넌트의 HTML Preview</p>
</div>

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- ...생략 -->
    <script
      src="/_next/static/chunks/pages/temp.js?ts=1659794015718"
      defer=""
    ></script>
  </head>
  <body>
    <div id="__next">
      <div class="temp__StyledContainer-sc-20bb4299-0 gWoQiG">
        Temp page: <span style="color:blue">howdy-mj</span>
      </div>
    </div>
    <!-- 생략 -->
    <script id="__NEXT_DATA__" type="application/json">
      {
        "props": {
          "pageProps": {
            "name": "howdy-mj"
          },
          "__N_SSP": true
        },
        "page": "/temp",
        "query": {},
        "buildId": "development",
        "isFallback": false,
        "gssp": true,
        "scriptLoader": []
      }
    </script>
  </body>
</html>
```

서버에서 렌더링한 HTML에 `getServerSideProps()`로 불러온 데이터와 CSS도 같이 반환되는 것을 확인할 수 있다. 이 외, styled-components로 생성한 스타일과 React 내부에서 선언한 state 그리고 onClick 메서드는 `temp.js` 파일에서 확인할 수 있다.

브라우저에 렌더링된 화면은 DOM에 자바스크립트 코드를 서버에서 가져온 HTML에 연결하여 작성한 코드가 모두 정상 동작한다.

<!-- <br> -->

<!-- ### 궁금점

Next.js의 package.json을 살펴보니, 스크립트 실행을 <a href="https://github.com/lukeed/taskr" target="_blank">taskr</a> 라이브러리로 실행하고 있다.

```json
{
  // ...
  "scripts": {
    "dev": "taskr",
    "release": "taskr release",
    "build": "pnpm release && pnpm types",
    "prepublishOnly": "cd ../../ && turbo run build",
    "types": "tsc --declaration --emitDeclarationOnly --declarationDir dist",
    "typescript": "tsec --noEmit",
    "ncc-compiled": "ncc cache clean && taskr ncc"
  }
}
```

마지막 릴리즈가 2017년도로, 더 이상의 유지보수는 하지 않는 것으로 보이는데 계속 사용하는 이유가 뭘까?라는 궁금증에 남겨둔다. -->

<br />

**참고**

<div style="font-size: 12px;">

- <a href="https://nextjs.org/" target="_blank">Next.js</a>

- <a href="https://reactjs.org/" target="_blank">React</a>

- <a href="https://milban.dev/SPA%EA%B8%B0%EB%B0%98%20SSR%20%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0%20(feat.React)%203-SSR%EC%9A%A9%20%EC%84%9C%EB%B2%84%20%EA%B5%AC%ED%98%84%EA%B3%BC%20Hydration/" target="_blank">SPA기반 SSR 구현하기 (feat.React) 3-SSR용 서버 구현과 Hydration</a>

<!-- - <a href="https://github.com/vercel/next.js/discussions/22276" target="_blank">What is a technical definition of "hydration" within NextJS?</a> -->

</div>
