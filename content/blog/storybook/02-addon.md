---
title: 'Storybook: addon'
date: 2020-6-3 19:10:00
category: 'storybook'
draft: true
---

## 1. addon이란?

addon은 Storybook의 plugin과 비슷한 역할로 고급 기능을 지원하고 새로운 워크플로우를 제공해준다.

대표적인 addon은 Knobs, Actions, Source, Docs, Viewport, Storyshots, Backgrounds, Accessibility, Console, Links가 있으며 더 자세한 내용은 [홈페이지](https://storybook.js.org/addons/)에서 확인 가능하다.

<br>

이번 글에서는 Actions, Knobs, Docs의 정의와 기본 세팅에 알아 보겠다.

### Actions

원래 Actions도 따로 설치를 해야 보이는걸로 아는데, 업데이트 되면서 바뀌었는지 storybook을 설치하면 actinos도 같이 설치되어 있다.

> .storybook - main.js

```js
module.exports = {
  stories: ['../src/**/*.stories.js'],
  addons: [
    '@storybook/preset-create-react-app',
    '@storybook/addon-actions',
    '@storybook/addon-links',
  ],
}
```

> package.json

```json{5}
...,
"devDependencies": {
    "@storybook/addon-actions": "^5.3.19",
    "@storybook/addon-links": "^5.3.19",
    "@storybook/addons": "^5.3.19",
    "@storybook/preset-create-react-app": "^3.0.0",
    "@storybook/react": "^5.3.19"
}
```

우선 storybook에 기본으로 세팅되어 있는 Button을 보자.

```
src
└── stories
    ├── 0-Weclome.stories.js
    └── 1-Button.sotries.js
```

버튼을 클릭하게 되면 아래 `Actions`탭에 clicked한 내용이 뜬다.

![](./images/02-01.png)

더 정확하게는, Actions는 Componenet를 통해 특정 함수가 호출 되었을 때, 어떤 함수가 어떤 parameter를 넣어서 호출되었는지에 대한 정보를 알려준다.
