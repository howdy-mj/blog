---
title: 'useEffect() 톺아보기'
date: 2021-2-21 02:42:23
category: 'react'
draft: true
---

## useEffect() 톺아보기

```js
export function useEffect(
  create: () => (() => void) | void,
  deps: Array<mixed> | void | null
): void {
  const dispatcher = resolveDispatcher()
  return dispatcher.useEffect(create, deps)
}
```
