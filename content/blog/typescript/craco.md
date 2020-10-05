---
title: 'Typescript 절대경로 설정'
date: 2020-9-24 12:03:56
category: 'typescript'
draft: true
---

### 절대경로 설정

폴더 구조가 겹겹인 경우, import 경로를 상대경로로 하기에 한계가 있다. 해당 프로젝트에서는 굳이 절대경로를 사용하지 않아도 되지만, 연습 차원에서 사용해보겠다.

절대 경로를 사용하기 위해서는 `tsconfig.json`과 webpack 설정을 바꾸면 되지만, `tsconfig.json`에서 paths를 수정하고 `yarn start`를 하면 강제적으로 작성한 paths가 없어진다... (이거 때문에 3시간 헤맴ㅠㅠ)

이유를 찾아보니 CRA로 만든 프로젝트는 eject를 해서 숨겨진 webpack 설정을 바꾸어야 하는데, 그러기엔 CRA로 만든 장점이 없어지기 때문에 [craco-alias](https://www.npmjs.com/package/craco-alias)를 설치하겠다.

```shell
$ yarn add craco-alias -D
```

설치 후, root 폴더에서 아래의 파일을 수정 생성하자.

`package.json`

```json
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject",
    "storybook": "start-storybook -p 6006 -s public",
    "build-storybook": "build-storybook -s public"
  },
```

`craco.config.js`

```js
const CracoAlias = require('craco-alias')

module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: 'tsconfig',
        jsConfigPath: 'tsconfig.paths.json',
      },
    },
  ],
}
```
