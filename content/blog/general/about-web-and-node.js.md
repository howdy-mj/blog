---
title: '브라우저 동작 원리와 Node.js'
date: 2020-6-5 11:10:00
category: 'general'
draft: true
---

## Node.js란?

대부분 npm 설명은 아래처럼 되어 있다.

- npm은 Node Package Manager의 줄임말로 Node.js 패키지 관리를 원활하게 도와주는 것이다.
- npm은 Node.js의 패키지 생태계이면서 세계에서 가장 큰 오픈 소스 라이브러리이다.

그렇다면 Node.js란 무엇일까?

[공식 홈페이지](https://nodejs.org/en/about/)에는 이렇게 설명되어있다.
Node.js®는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임이다.Node.js는 이벤트 기반, Non-Blocking I/O 모델을 사용해 가볍고 효율적이다.

단어 하나하나 살펴보자.

**V8**
[V8](https://v8.dev/)은 구글의 오픈소스

V8 is Google’s open source high-performance JavaScript and WebAssembly engine, written in C++. It is used in Chrome and in Node.js, among others. It implements ECMAScript and WebAssembly, and runs on Windows 7 or later, macOS 10.12+, and Linux systems that use x64, IA-32, ARM, or MIPS processors. V8 can run standalone, or can be embedded into any C++ application.

**JavaScript 엔진**
JavaScript 엔진

**런타임(runtime)**
프로그램이 실행되고 있을 때 존재하는 곳을 말한다.
즉 컴퓨터 내에서 프로그램이 기동되면, 그것이 바로 그 프로그램의 런타임이다.

**스레드**
스레드

블로킹
논-블로킹

==
Node.js는 구글 크롬의 V8 자바스크립트 엔진에 기반해 만들어진 자바스크립트 런타임 환경(Runtime Enviroment)로, 자바스크립트는 웹 브라우저를 벗어나 서버 사이드 애플리케이션 개발에서도 사용되는 범용 개발언어가 되었다.

> <span style="font-size: 12px;">(\*서버 사이드 렌더링, SSR: 브라우저에 나타나는 형태 그대로를 HTML로 만들어 제공)</span>

**참고**

- https://junspapa-itdev.tistory.com/3
