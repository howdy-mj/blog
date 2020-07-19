---
title: 'React HOC(Higher-Order Components)'
date: 2020-7-19 20:10:43
category: 'React'
draft: true
---

## HOC(Higher-Order Components)란?

Hight-Order Components HOC는 컴포넌트 로직을 재사용하기 위한 리액트의 고급 기술이다. 함수로 컴포넌트를 인자로 받아 새로운 컴포넌트를 리턴한다.

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent)
```

기존 컴포넌트가 props를 받아 UI를 만들었다면, HOC는 컴포넌트를 다른 컴포넌트로 바꾼다.

말이 좀 어려운데, 쉽게 말하자면 자주 반복하는 코드를 하나의 함수를 만들어 여기저기서 재사용 가능한 컴포넌트를 말한다.

예를 들어, 로그인 여부에 따른 페이지 구현이나 로딩 중 화면 표시 등과 같이 여러 페이지에서 공통으로 쓰이는 것에 활용할 수 있다.

**참고**

<div style="font-size: 12px;">

- https://reactjs.org/docs/higher-order-components.html

</div>
