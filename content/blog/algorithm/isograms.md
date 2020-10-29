---
title: 'Isograms'
date: 2020-10-30 00:42:29
category: 'algorithm'
draft: false
---

## 문제

An isogram is a word that has no repeating letters, consecutive or non-consecutive. Implement a function that determines whether a string that contains only letters is an isogram. Assume the empty string is an isogram. Ignore letter case.

```js
isIsogram('Dermatoglyphics') == true
isIsogram('aba') == false
isIsogram('moOse') == false // -- ignore letter case
```

## 내가 푼 방식

`2020.10.30`

```js
function isIsogram(str) {
  const lowerStr = str.toLowerCase()

  for (let i = 0; i < lowerStr.length; i++) {
    for (let j = i + 1; j < lowerStr.length; j++) {
      if (lowerStr[i] === lowerStr[j]) {
        return false
      }
    }
  }
  return true
}
```

<br />

출처: [CodeWars](https://www.codewars.com/)
