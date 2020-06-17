---
title: 'Webpack 웹팩 알아보기(2) : plugin'
date: 2020-6-16 23:38:13
category: 'Node.js'
draft: true
---

## 플러그인(Plugin)

로더가 파일 단위로 처리하는 반면, 플러그인은 번들된 결과물을 처리한다. 번들된 자바스크립트를 난독화 한다거나 특정 텍스트를 추출하는 용도로 사용한다.

공식홈페이지의 [Basic Plugin Architecture](https://webpack.js.org/contribute/writing-a-plugin/)를 따라해보자.

`hello-world-plugin.js`

```js
class HelloWorldPlugin {
  apply(compiler) {
    // plugin이 종료 되었을 때 실행된다.
    compiler.hooks.done.tap('Hello World Plugin', stats => {
      console.log('Hello World!')
    })
  }
}

module.exports = HelloWorldPlugin
```

`webpack.config.js`

```js
const HelloWorldPlugin = require('./hello-world-plugin')

module.exports = {
  plugins: [new HelloWorldPlugin()],
}
```

설정을 마치고 `npm run build`를 하면 'Hello World!'가 찍힌 것을 볼 수 있다.

```sh{3}
$ npm run build

Hello World!
Hash: cabe67579d243965d759
Version: webpack 4.43.0
Time: 1349ms
Built at: 2020-06-15 23:25:32
                                  Asset      Size  Chunks             Chunk Names
bg.png?5c6d3b633991b51295c68b34d8b94c8b  1.17 MiB          [emitted]
                                main.js    22 KiB    main  [emitted]  main
Entrypoint main = main.js
[./node_modules/css-loader/dist/cjs.js!./src/app.css] 581 bytes {main} [built]
[./src/app.css] 517 bytes {main} [built]
[./src/app.js] 186 bytes {main} [built]
[./src/bg.png] 64 bytes {main} [built]
[./src/howdy.png] 2.93 KiB {main} [built]
    + 3 hidden modules
```

<br />

**참고**

<div style="font-size: 12px;">

- https://webpack.js.org/
- https://jeonghwan-kim.github.io/series/2019/12/10/frontend-dev-env-webpack-basic.html
- https://joshua1988.github.io/webpack-guide

</div>
