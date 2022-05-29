---
title: 'CSS의 우선순위와 캐스케이드'
date: 2022-6-1 00:00:00
category: 'css'
draft: false
---

CSS 완벽 가이드

CSS는 Cascading Style Sheets의 약자이다.

상속(inheritance)은 프로퍼티 값이 요소에서 그 자손으로 전달되는 매커니즘이다. 어떤 값을 요소에 적용해야 하는지 결정할 때, 사용자 에이전트는 상속뿐만 아니라 선언의 우선순위(specificity)와 선언의 출처를 고려해야 한다. 이 결정 과정을 캐스케이드(cascade)라 부른다.

우성 상속과 캐스케이드가 어떻게 다른지는 이렇게 요약할 수 있다.

```css
ht {
  color: red;
  color: blue;
}
```

이 결과를 결정하는 것은 캐스케이드이고, h1 안에 있는 span 요소를 파란 글씨로 만드는 것이 상속이다.

https://css-tricks.com/css-cascade-layers/

<br>

**참고**

<div style="font-size: 12px;">

- <a href="https://drafts.csswg.org/css-cascade/" target="_blank">CSS Cascading and Inheritance Level 4 (Editor's Draft, 2022.04.14)</a>

- <a href="https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance" target="_blank">계단식 및 상속 | MDN</a>

- <a href="" target="_blank"></a>

</div>
