---
title: '스토리북으로 Stories 작성하기'
date: 2020-11-30 23:03:56
category: 'storybook'
draft: false
---

스토리북에서 stories를 어떻게 작성하는지 알아보자.

해당 글은 react `^17.0.1`, react-scripts `4.0.1`, @storybook/react `^6.1.8`, styled-components `^5.2.1`, typescript `^4.0.3` 버전으로 작성되었다.

완성된 코드: [Github - writing-stories](https://github.com/howdy-mj/writing-stories)

## 프로젝트 세팅

```shell
$ yarn create react-app writing-stories --template typescript
$ cd writing-stories
$ npx sb init
$ yarn add styled-components
$ yarn add -D @types/styled-components
$ yarn storybook # 스토리북 서버
```

<div style="text-align: center; font-size: 14px;"">
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b9411bd7-1218-4ed9-b7a0-b4bb5e831f9d/_2020-11-30__9.21.22.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20201130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201130T122148Z&X-Amz-Expires=86400&X-Amz-Signature=e87da5acad2a32ae673b70708244fd619dff3dd93679cfdb940b2d19aba14cde&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2020-11-30__9.21.22.png%22" alt="폴더 구조">
<div>기존 폴더 구조</div>
</div>

기본 구조는 위와 같아지는데, 개인적으로 stories와 컴포넌트가 같이 있는 것을 선호하기 때문에 약간의 폴더 구조 변경을 하며, 해당 글은 오로지 Storybook에만 치중하기 때문에, `src/index.tsx`와 `src/App.tsx`는 건드리지 않으며, 예제는 기본으로 내제된 컴포넌트를 활용하겠다.

## 폴더 구조 변경

<div style="text-align: center; font-size: 14px;">
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8c13592a-12d7-4f41-a8c4-e287a8f2cac4/_2020-11-30__10.20.53.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20201130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201130T132126Z&X-Amz-Expires=86400&X-Amz-Signature=7740132c26519534ee8723aca039a9bdff78f2633b2558ca3c0bd971269c59f7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2020-11-30__10.20.53.png%22" alt="변경한 폴더 구조">
<div>변경한 폴더 구조</div>
</div>

기존의 `Button.tsx`와 `Header.tsx`는 components/Button, components/Header 안으로, `Pages.tsx`는 pages/Main으로 변경했으며, 기존의 css는 모두 styled-components로 변경했다. [Github: Folder Structure](https://github.com/howdy-mj/writing-stories/commit/8b9403948f9102f6adc23f166667ed1ccd5a0ed9)

```shell
$ yarn storybook
```

이 후 다시 스토리북을 실행하면 이전과 같은 화면을 볼 수 있다.

해당 글에서는 Button 컴포넌트를 다루어 보겠다.

<div style="text-align: center; font-size: 14px;">
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b846e121-68e4-4f53-b6b1-6d7b7d5bd565/_2020-11-30__10.27.17.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20201130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201130T133120Z&X-Amz-Expires=86400&X-Amz-Signature=5e41ece0a90a4c72c0dedd7d4abef654e443a7245d59e86077cc97800f6b2b00&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2020-11-30__10.27.17.png%22" alt="Button Docs">
<div>Button Docs 화면</div>
</div>

개인적으로 (현재의 나로서는) 스토리북의 최대 장점은 Docs라 생각한다. Storybook 6로 오면서 TypeScript 지원이 훨씬 간단해졌으며, interface를 지정해주고, 주석으로 달아주면 위 이미지처럼 스토리북 Docs에서 그대로 볼 수 있다.

문서화를 함으로써, 만들어 둔 컴포넌트에 어떤 props가 있는지, 어떻게 사용해야 하는지 굳이 알려줄 필요 없이 스토리북 서버에서 바로 확인 가능하다.

`components/Button/index.tsx`

```ts
export interface ButtonProps {
  /**
   * Is this the principal call to action on the page?
   */
  primary?: boolean
  /**
   * What background color to use
   */
  bgColor?: string
  /**
   * How large should the button be?
   */
  size?: 'small' | 'medium' | 'large'
  /**
   * Button contents
   */
  label: string
  /**
   * Optional click handler
   */
  onClick?: () => void
}

/**
 * Primary UI component for user interaction
 */
// => 스토리북 Docs 소제목으로 들어감
export const Button: React.FC<ButtonProps> = ({
  primary = false,
  size = 'medium',
  bgColor,
  label,
  ...props
}) => {
  return (
    <ButtonWrap
      type="button"
      primary={primary}
      style={{ backgroundColor: bgColor }}
      size={size}
      {...props}
    >
      {label}
    </ButtonWrap>
  )
}
```

<div style="text-align: center; font-size: 14px;">
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7a83c43c-6caa-449f-931f-57afc088c8fc/_2020-11-30__10.26.59.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20201130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201130T132728Z&X-Amz-Expires=86400&X-Amz-Signature=7f4828fa7e12e5c26c83d72c78b0e848126779007e6e81a42bff76ff55e433b8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22_2020-11-30__10.26.59.png%22" alt="Button stories">
<div>Button Stories 화면</div>
</div>

그리고 해당 props일 때 어떤 UI를 가지는지도 스토리북에서 바로 확인이 가능하다.

`components/Button/Button.stories.tsx`

```ts
import React from 'react'
// also exported from '@storybook/react' if you can deal with breaking changes in 6.1
import { Story, Meta } from '@storybook/react/types-6-0'

import { Button, ButtonProps } from './index'

export default {
  title: 'Components/Button',
  component: Button,
  argTypes: {
    backgroundColor: { control: 'color' },
  },
} as Meta

const Template: Story<ButtonProps> = args => <Button {...args} />

export const Primary = Template.bind({})
Primary.args = {
  primary: true,
  label: 'Button',
}

export const Secondary = Template.bind({})
Secondary.args = {
  label: 'Button',
}

export const Large = Template.bind({})
Large.args = {
  size: 'large',
  label: 'Button',
}

export const Small = Template.bind({})
Small.args = {
  size: 'small',
  label: 'Button',
}
```

_추후 내용 추가 예정_

<br />

**참고**

<div style="font-size: 12px;">

- https://storybook.js.org/

<div>
