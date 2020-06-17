---
title: 'React HOC(Higher-Order Components)'
date: 2020-6-17 01:10:43
category: 'React'
draft: true
---

## HOC(Higher-Order Components)란?

Hight-Order Components HOC는 컴포넌트 로직을 재사용하기 위한 리액트의 고급 기술이다. 함수로 컴포넌트를 인자로 받아 새로운 컴포넌트를 리턴한다.

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent)
```

기존 컴포넌트가 props를 받아 UI를 만들었다면, HOC는 컴포넌트를 다른 컴포넌트로 바꾼다.

**참고**

<div style="font-size: 12px;">

- https://reactjs.org/docs/higher-order-components.html

</div>
