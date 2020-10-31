---
title: 'Stop gninnipS My sdroW!'
date: 2020-11-01 00:18:29
category: 'algorithm'
draft: false
---

## 문제

Write a function that takes in a string of one or more words, and returns the same string, but with all five or more letter words reversed (Just like the name of this Kata). Strings passed in will consist of only letters and spaces. Spaces will be included only when more than one word is present.

Examples:

```
spinWords( "Hey fellow warriors" ) => returns "Hey wollef sroirraw"
spinWords( "This is a test") => returns "This is a test"
spinWords( "This is another test" ) => returns "This is rehtona test"
```

## 내 풀이

2020.11.01 (`20m`)

```js
function spinWords(str) {
  let result = ''
  const arr = str.split(' ')
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].length <= 4) {
      result += arr[i] + ' '
    } else {
      result +=
        arr[i]
          .split('')
          .reverse()
          .join('') + ' '
    }
  }
  return result.substring(0, result.length - 1)
}
```

<br />

출처: [CodeWars](https://www.codewars.com/)
