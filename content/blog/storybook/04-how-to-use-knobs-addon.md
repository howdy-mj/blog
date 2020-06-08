---
title: 'Storybook: Knobs addon 사용법'
date: 2020-6-10 01:04:14
category: 'storybook'
draft: true
---

해당 글의 모든 예시는 기본으로 설치되어 있는 `src/storeis/1-Button.stories.js`에서 추가하는 형태로 진행하겠다.

## Knobs

- [Knobs repo](https://github.com/storybookjs/storybook/tree/master/addons/knobs)
- [Knobs 소개 및 설치](https://howdy-mj.netlify.app/storybook/02-addon-intro/#knobs)

Knobs는 component에 props를 추가해주는 것이다.

```js
import { withKnobs, text, boolean, select } from '@storybook/addon-knobs'

export default {
  title: 'Button',
  component: Button,
  decorators: [withKnobs],
  // storeis에서 knobs를 지원하도록 'withKnobs'라는 decorator를 추가해준다.
}
```
