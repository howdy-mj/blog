---
title: 'Storybook: Knobs addon 사용법'
date: 2020-6-10 01:04:14
category: 'storybook'
draft: true
---

해당 글의 모든 예시는 기본으로 설치되어 있는 `src/storeis/1-Button.stories.js`의 `Emoji`는 삭제하고, `Text`에서 추가하는 형태로 진행하겠다.

## Knobs

- [Knobs repo](https://github.com/storybookjs/storybook/tree/master/addons/knobs)
- [Knobs 소개 및 설치](https://howdy-mj.netlify.app/storybook/02-addon-intro/#knobs)

Knobs는 component에 props가 있을 때, 더 보기 쉽게 도와주는 addon이다.

기존에 `1-Button.stories.js`에서 쓴 것을, `1-Button.js`에 Button 컴포넌트만 쓰고 import해오는 것으로 바꿔보겠다.

```
src
└─ stories
    ├─ 0-Weclome.stories.js
    ├─ 1-Button.css
    ├─ 1-Button.js
    └─ 1-Button.sotries.js
```

`src/stories/1-Button.js`

```jsx
import React from 'react'
import './1-Button.css'

const Button = ({ disabled, name, clicked }) => {
  return (
    <button className="Button" onClick={clicked} disabled={disabled}>
      Hi {name}
    </button>
  )
}

export default Button
```

`src/stories/1-Button.css`

```css
.Button {
  padding: 10px;
  width: 100px;
  background-color: aliceblue;
  border-radius: 5px;
  border: none;
}
```

`src/stories/1-Button.stories.js`

```jsx
import React from 'react'
import { action } from '@storybook/addon-actions'
import { withKnobs, text, boolean } from '@storybook/addon-knobs'
import Button from './1-Button'

export default {
  title: 'Button', // 스토리북의 그룹과 경로
  component: Button, // 어떤 컴포넌트를 문서화 할지
  decorators: [withKnobs], // knobs 적용
}

export const buttonText = () => {
  // knobs 만들기
  const name = text('name', '버튼')
  const disabled = boolean('disabled', false)

  return <Button clicked={action('clicked')} disabled={disabled} name={name} />
}
```
