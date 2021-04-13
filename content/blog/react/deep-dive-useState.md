---
title: 'useState() 톺아보기'
date: 2021-2-21 02:42:23
category: 'react'
draft: true
---

React를 쓰고 있다면 state를 변경하기 위해 useState (혹은 setState)를 사용하고 있을 것이다.

하지만 필자는 정작 useState가 정확히 어떤 동작 원리인지 몰랐기에, 이번 기회에 React의 useState 코드 분석을 해보고자 한다.

```js
const [name, setName] = useState('kmj')
```

name이라는 값의 초기값은 'kmj'이고, setName()을 통해 해당 값을 바꿀 수 있다.

useState()는 배열을 비구조화 할당(array destructuring)한 것이다.

```js
function mockUseState(initialState) {
  return [initialState, newValue => mockUseState(newValue)]
  // 두번 째 인자에는 initialState를 바꾸는 함수가 들어간다
}

mockUseState('kmj') // output: ['kmj', f ()]
```

### useState 코드

이제 useState()를 살펴보자.

```js
// react/packages/react/src/ReactHooks.js
export function useState<S>(
  initialState: (() => S) | S
): [S, Dispatch<BasicStateAction<S>>] {
  const dispatcher = resolveDispatcher()
  return dispatcher.useState(initialState)
}
```

처음에 initialState를 받아서 배열의 첫 번째 인자에 넣고, 두 번째가 이를 바꾸어주는 함수로 보인다. `Dispatch`, `BasicStateAction`을 살펴보자.

```js
// react/packages/react/src/ReactHooks.js
type BasicStateAction<S> = (S => S) | S
type Dispatch<A> = A => void
```

받은 인자를 전달하여 값을 바꾸는 것으로 보인다. 위에 useState()로 선언한 name을 풀어보면 아래와 같이 작성할 수 있을 것 같다.

```js
const mockUseState = value => {
  console.log('mockUseState value:', value)
  return [value, newValue => mockUseState(newValue)]
}

const [name, setName] = mockUseState('kmj')
console.log('init: ', name)

setName('howdy')
setTimeout(() => {
  console.log('after change: ', name)
}, 1500)
```

위의 console 결과는 아래와 같다.

```
mockUseState value: kmj
init:  kmj
mockUseState value: howdy
after change: kmj
```

분명 mockUseState에서 바뀐 값이 들어갔는데 실제 name은 바뀌지 않았다. (추후 수정..ㅠㅠ) 하지만 대충 useState()의 구조가 이와 비슷하다는 걸 유추해볼 수 있었다.

### 바닥부터 살펴보기

그렇다면 `resolveDispatcher()`는 무엇일까?

```js
// react/packages/react/src/ReactHooks.js
function resolveDispatcher() {
  const dispatcher = ReactCurrentDispatcher.current
  if (__DEV__) {
    if (dispatcher === null) {
      console.error(
        'Invalid hook call. Hooks can only be called inside of the body of a function component. This could happen for' +
          ' one of the following reasons:\n' +
          '1. You might have mismatching versions of React and the renderer (such as React DOM)\n' +
          '2. You might be breaking the Rules of Hooks\n' +
          '3. You might have more than one copy of React in the same app\n' +
          'See https://reactjs.org/link/invalid-hook-call for tips about how to debug and fix this problem.'
      )
    }
  }
  // Will result in a null access error if accessed outside render phase. We
  // intentionally don't throw our own error because this is in a hot path.
  // Also helps ensure this is inlined.
  return ((dispatcher: any): Dispatcher)
}
```

```js
// react/packages/react/src/ReactCurrentDispatcher.js
import type { Dispatcher } from 'react-reconciler/src/ReactInternalTypes'

const ReactCurrentDispatcher = {
  /**
   * @internal
   * @type {ReactComponent}
   */
  current: (null: null | Dispatcher),
}

export default ReactCurrentDispatcher
```

Dispatcher는 react-reconciler에서 가져오는 걸 확인하여 다시 올라가보자(!)

```js
// https://github.com/facebook/react/blob/090c6ed7515b63c9aa7c42659973bfc179691006/packages/react-reconciler/src/ReactInternalTypes.js#L267

export type Dispatcher = {|
  readContext<T>(
    context: ReactContext<T>,
    observedBits: void | number | boolean
  ): T,
  useState<S>(initialState: (() => S) | S): [S, Dispatch<BasicStateAction<S>>],
  // ...생략

  unstable_isNewReconciler?: boolean,
|}

// https://github.com/facebook/react/blob/090c6ed7515b63c9aa7c42659973bfc179691006/packages/react-reconciler/src/ReactFiberHooks.new.js#L622
function basicStateReducer<S>(state: S, action: BasicStateAction<S>): S {
  // $FlowFixMe: Flow doesn't like mixed types
  return typeof action === 'function' ? action(state) : action
}
```

https://www.npmjs.com/package/react-reconciler

https://github.com/facebook/react/blob/83381c1673d14cd16cf747e34c945291e5518a86/src/renderers/shared/stack/reconciler/ReactReconciler.js

```js
// https://github.com/facebook/react/blob/090c6ed7515b63c9aa7c42659973bfc179691006/packages/react-reconciler/src/ReactFiberHooks.new.js#L1938
useState<S>(
      initialState: (() => S) | S,
    ): [S, Dispatch<BasicStateAction<S>>] {
      currentHookNameInDev = 'useState';
      mountHookTypesDev();
      const prevDispatcher = ReactCurrentDispatcher.current;
      ReactCurrentDispatcher.current = InvalidNestedHooksDispatcherOnMountInDEV;
      try {
        return mountState(initialState);
      } finally {
        ReactCurrentDispatcher.current = prevDispatcher;
      }
    },


// https://github.com/facebook/react/blob/090c6ed7515b63c9aa7c42659973bfc179691006/packages/react-reconciler/src/ReactFiberHooks.new.js#L2070
useState<S>(
      initialState: (() => S) | S,
    ): [S, Dispatch<BasicStateAction<S>>] {
      currentHookNameInDev = 'useState';
      updateHookTypesDev();
      const prevDispatcher = ReactCurrentDispatcher.current;
      ReactCurrentDispatcher.current = InvalidNestedHooksDispatcherOnMountInDEV;
      try {
        return mountState(initialState);
      } finally {
        ReactCurrentDispatcher.current = prevDispatcher;
      }
    },


```

<br />

**참고**

<div style="font-size: 12px;">

- https://github.com/facebook/react/blob/master/packages/react/src/ReactHooks.js

- https://github.com/facebook/react/blob/master/packages/react/src/ReactCurrentDispatcher.js

- https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/

- https://medium.com/react-native-seoul/react-%EB%A6%AC%EC%95%A1%ED%8A%B8%EB%A5%BC-%EC%B2%98%EC%9D%8C%EB%B6%80%ED%84%B0-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-04-functional-component%EC%99%80-usestate-e70b60962f95

</div>
