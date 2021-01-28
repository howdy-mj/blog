---
title: 'HTML 살펴보기'
date: 2021-01-23 01:58:00
category: 'html'
draft: true
---

Web Storage, Socket API도 HTML 스펙안에 있음

브라우저가 공식적으로 지원하는 언어 4종류에 HTML, CSS, JAvaScript, WASM이 있음

HTML을 잘 안다는 건, 브라우저가 어떤 방식으로 동작하는 이해한다.
HTML을 잘 안다는 건, 검색엔진, 크롤러, 콘텐츠 표현 방식 (시맨틱), 접근성 등에 대한 이해도를 높인다.

---

웹 표준(web standard): <section> 요소를 사용했을 때 모든 브라우저에서 동일한 동작을 기대할 수 있어야 한다
웹 표준 주요 기구는 W3C, WHATWG 등이 있음

Structural languages

- HTML, XML

Presentation languages

- CSS1, CSS2, XSL (CSS3은 다른 카테고리)

Object models

- DOM 1 core HTML/XML

Scripting

- ECMAScript

---

1998년에 웹 표준 프로젝트가 시작, 2013년에 마무리

---

웹 콘텐츠 접근성

- Accessiblity, 접근성, !== 반드시 장애인을 대상으로 하는 것은 아님
- 접근성이 반드시 추가 작업을 일으키거나, 장애인만을 대상으로 하는게 아님
- 모두가 웹에 있는 콘텐츠에 잘 접근할 수 있어야 한다.

## WCAG(Web Content Accessbility Guidline)

- W3C에서 관리하고 있음
- 각각의 항목이 개발자만 봐서 해결되는 것이 아니라, 프로덕트를 만드는 사람 모두가 함께 챙겨야 함
- Best Practice를 잘 준수하고, HTML 표준을 잘 준수하는 것도 WCAG에 포함

* Perceivable
* Operable
* Understandable
* Robust

## KWCAG (최신은 2.1, 한국의 웹 표준)

인식의 용이성
운용의 용이성
이해의 용이성
견고성
