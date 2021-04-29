---
title: '리액티브 프로그래밍과 RxJS'
date: 2021-5-1 00:00:00
category: 'rxjs'
draft: false
---

대부분 어플리케이션에는 비동기 작업 처리가 필수일 것이다. 그리고 그 어플리케이션의 규모가 커지면 callback, Promise, async/await로만 그 많은 양의 데이터를 처리하기에는 코드가 복잡해지고 가독성이 떨어질 수 밖에 없다.

이런 복잡한 데이터를 가공하고 처리하기 위해 <span class="definition">리액티브 프로그래밍(Reactive Programming)</span>이 자연스레 나온게 아닐까 추측한다.

<br />

RxJS의 컨트리뷰터 <a href="https://gist.github.com/staltz/868e7e9bc2a7b8c1f754" target="_blank">André Staltz</a>는 리액티브 프로그래밍을 <span class="variable bold">Reactive programming is programming with asynchronous data streams</span>, 즉 비동기 데이터 스트림을 이용한 프로그래밍이라 설명하고 있다.

여기서 스트림은 '순서대로 진행중인 이벤트'라고 말할 수 있다.

선언형 프로그래밍과 같은 방식..

---

RxJS(Reactive Extensions for JavaScript)는 파일 읽기, HTTP 호출, 키 입력 또는 마우스 움직임 등 흔한 이벤트의 소스를 처리하는 단일 프로그래밍을 사용하여 콜백 또는 Promise 기반 라이브러리를 정확히 같은 방식으로 대체합니다. 예를 들어 콜백으로 각 마우스 이벤트를 독립적으로 처리하는 대신에 RxJS로 모든 이벤트를 결합하여 처리합니다.

RxJS에서 데이터 스트림(data stream)이란 키 입력, 움직임 이벤트, 터치 동작, 원격 HTTP 호출, 단일 정수 등의 처리를 지칭한다.

--

생산자(Producer)는 데이터의 소스입니다. 스트림에는 항상 데이터 생산자가 있어야 하며, RxJS에서 수행할 모든 로직의 시작점이 됩니다. 실제로 생산자는 독립적으로 이벤트를 생성하는 무언가(단일 값, 배열, 마우스 클릭, 파일로부터 읽어온 바이트 스트림에 이르기까지)에서 생성됩니다. 이러한 생산자를 옵저버 패턴에서는 서브젝트(Subject)라고 정의하고, RxJS에는 관찰될 수 있는 무언가라는 의미로 옵저버블(Observable)이라 부릅니다

옵저버블은 알림을 푸시하는 역할만 해서 이 동작을 실행 후 잊기(fire-and-forget)라 부릅니다. 즉, 생산자는 이벤트 방출에만 관여하고 이벤트 처리에는 관여하지 않습니다.

--

옵저버블은 구독되기 전까지 동작하지 않는데, 이러한 특성을 갖는 옵저버블을 cold observable이라 한다. rxjs의 옵저버블은 기본적으로 cold observable이다. 이는 구독되기 전에는 데이터 스트림을 방출(emit)하지 않으며 구독하면 처음부터 동작하기 시작한다. 따라서 옵저버는 옵저버블이 방출하는 모든 데이터 스트림을 빠짐없이 처음부터 받을 수 있다.

---

## Observable vs. Promise

- 옵저버블은 명시적으로 구독하기 전까지는 실행되지 않지만, Promise는 객체를 생성할 때 바로 실행된다. 데이터를 받는 쪽에서 원하는 시점을 결정하는 경우엔 옵저버블이 더 효율적이다.

- 옵저버블은 데이터를 여러 개 보낼 수 있지만, Promise는 하나만 보낼 수 있다. 데이터를 여러번 나눠서 보내는 경우라면 옵저버블이 더 효율적이다.

- 옵저버블은 체이닝과 구독을 구별하지만, Promise는 .then() 하나로 사용한다. 다른 곳에서 데이터를 복잡하게 가공해야 한다면 옵저버블이 더 효율적이다.

- 옵저버블에서 제공하는 subscribe()는 에러도 함께 처리할 수 있다. Promise는 .catch()를 사용하는 위치에 따라 에러를 처리하는 로직이 달라져야 하지만, 옵저버블은 에러 처리 로직을 한 군데에 집중할 수 있다.

<br />

**참고**

<div style="font-size: 12px;">

- http://reactivex.io/

- https://rxjs-dev.firebaseapp.com/guide/overview

- https://thebook.io/006934/part01/ch01/04/03-02/

* https://feel5ny.github.io/2018/03/25/angular_observable/

* https://poiemaweb.com/angular-rxjs-observable

* https://angular.kr/guide/comparing-observables

</div>
