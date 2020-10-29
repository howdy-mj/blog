---
title: 'CRA에서 eject 없이 Less 사용하기'
date: 2020-10-26 23:42:29
category: 'css'
draft: true
---

구글링하면 대부분 eject를 하라고 하지만, CRA의 이점을 포기하기에는 너무 아깝다.
따라서 필자는 craco를 이용하여 웹팩 설정을 덮어씌우겠다.

```shell
$ yarn add @craco/craco craco-less
```

`packaage.json`

```json
// ...
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
// ...
```

root

`craco.config.js`

```js
const CracoLessPlugin = require('craco-less')

module.exports = {
  plugins: [{ plugin: CracoLessPlugin }],
}
```

https://www.syncfusion.com/blogs/post/less-versus-css.aspx
