---
title: 'CSS vs. Less vs. SASS(SCSS)'
date: 2020-10-24 23:42:29
category: 'css'
draft: true
---

## CSS

### CSS Preporcessor란?

CSS preprocessor(전처리)는 CSS에서 확장된 스크립팅 언어로 일반적인 CSS로 컴파일 해준다. 그 중 가장 유명한 것에 Less, SASS(SCSS)가 있다.

CSS 전처리를 사용하면

1. 클린 코드, 재사용 가능한 변수들
2. 시간 절약
3. 유지 보수가 용이함
4. 계산과 로직 설계 가능
5. 더 정돈된 느낌

## Less & SASS(SCSS)

두 가지 모두 아주 좋은 CSS extensions이다.

### 변수

CSS에서는 변수 사용이 불가하지만, CSS 전처리에서는 사용가능하며 보통 color, font 등 재사용하는 것을 변수로 만든다.

Sass font example

```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

CSS output#

```css
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```

Less color example#

```css
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

#header {
  color: @light-blue;
}
```

CSS output#

```css
#header {
  color: #6c94be;
}
```

둘다 mixin 기능을 제공하여 스타일을 변수화하여 사용가능하다.

### mixin

mixin은 여러가지 프로퍼티들을 하나로 묶을 때 사용한다.

Sass border radius example#
In Sass, you use @mixin:

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius;
}

.box {
  @include border-radius(10px);
}
```

CSS output#

```css
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}
```

Less border property example#

```css
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered;
}

.post a {
  color: red;
  .bordered;
}
```

CSS output#

```css
#menu a {
  color: #111;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

.post a {
  color: red;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

### Nesting

CSS 보다 가장 좋은 점이 바로 Nesting이다.

Sass#

```css
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li {
    display: inline-block;
  }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

CSS output#

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

Less#

```css
#header {
  color: black;

  .navigation {
    font-size: 12px;
  }

  .logo {
    width: 300px;
  }
}
```

CSS output#

```css
#header {
  color: black;
}

#header .navigation {
  font-size: 12px;
}

#header .logo {
  width: 300px;
}
```

### import

일반 CSS도 `@import`를 통해 여러 파일을 가져올 수 있지만, 이는 추가적인 HTTP 요청을 생성한다는 문제가 있다. Sass와 Less는 이와 조그 ㅁ다르게, 새로운 HTTP 요청을 보내기보다 하나의 파일로 합친다. concatenation과 비슷

Sass#
Let says you have a file named \_sheet1.scss. In your base.scss you can include it at the top by using the following: @import 'sheet1'; There is no need to have the extension.

Less#

```css
// Variables
@themes: "../../src/themes";

// Usage
@import '@{themes}/tidal-wave.less';
```

### Extend

Sass#
In Sass, you use @extend

```scss
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}
```

CSS output#

```css
.message,
.success,
.error,
.warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

Less#

```css
/*In Less you use :extend */
nav ul {
  &:extend(.inline);
  background: blue;
}

.inline {
  color: red;
}
```

CSS output#

```css
nav ul {
  background: blue;
}

.inline,
nav ul {
  color: red;
}
```

### Operations

Sass grid calculation example#

```scss
.container {
  width: 100%;
}

article[role='main'] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role='complimentary'] {
  float: right;
  width: 300px / 960px * 100%;
}
```

CSS output#

```css
.container {
  width: 100%;
}

article[role='main'] {
  float: left;
  width: 62.5%;
}

aside[role='complimentary'] {
  float: right;
  width: 31.25%;
}
```

Less square root example#

```css
sqrt(18.6%)
```

CSS output#

```css
4.312771730569565%;
```

[ 참고 ]

- https://css-tricks.com/sass-vs-less/
- https://www.keycdn.com/blog/sass-vs-less
