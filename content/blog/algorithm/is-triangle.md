---
title: 'Is this a triangle?'
date: 2020-10-30 00:42:29
category: 'algorithm'
draft: false
---

## 문제

Implement a method that accepts 3 integer values a, b, c. The method should return true if a triangle can be built with the sides of given length and false in any other case.

(In this case, all triangles must have surface greater than 0 to be accepted).

## 내 풀이

`2020.10.30`: 10'

```js
function isTriangle(a, b, c) {
  const sortArr = [a, b, c].sort()

  if (sortArr[2] < sortArr[0] + sortArr[1]) return true

  return false
}
```

가장 긴 변의 길이가 나머지 두개의 변 길이 합 보다 작으면 삼각형 성립

<br />

출처: [CodeWars](https://www.codewars.com/)
