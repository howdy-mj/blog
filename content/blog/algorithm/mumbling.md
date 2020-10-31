---
title: 'Mumbling'
date: 2020-10-31 00:42:29
category: 'algorithm'
draft: false
---

## 문제

This time no story, no theory. The examples below show you how to write function `accum`:

Examples:

```
accum("abcd") -> "A-Bb-Ccc-Dddd"
accum("RqaEzty") -> "R-Qq-Aaa-Eeee-Zzzzz-Tttttt-Yyyyyyy"
accum("cwAt") -> "C-Ww-Aaa-Tttt"
```

The parameter of accum is a string which includes only letters from `a..z` and `A..Z`.

## 내 풀이

2020.10.31 (`20m`)

```js
function accum(s) {
  let result = ''

  for (i in s) {
    for (let j = 0; j <= i; j++) {
      if (j === 0) {
        result = result + s[i].toUpperCase()
      } else {
        result = result + s[i].toLowerCase()
      }
    }
    result = result + '-'
  }
  return result.substring(0, result.length - 1)
}
```

<br />

출처: [CodeWars](https://www.codewars.com/)
