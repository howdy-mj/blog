---
title: 'react-spring: Interpolation'
date: 2020-5-30 18:21:13
category: 'react-spring'
draft: true
---

더 빠르고 작은 용량을 사용한다.
interpolate()의 매개변수로 객체와 함수를 가진다.
객체의 키값으로 range, output 등을 가진다.
range와 output을 활용해 keyframe과 transform의 효과를 만들 수 있다.
콜백 함수로 원하는 형식의 값을 리턴할 수 있다.

react-spring에서 등장하는 interpolation이 무엇인지 간단하게 알아보자.
우선 필자는 그래픽, 통계, 컴퓨터 관련 전공이 아니기 때문에 설명이 정확하지 않을 수 있다. 하지만 interpolation이 대충 무엇인지 파악하는데는 충분할 거라 믿는다.

## Interpolation 단어적 의미

**interpolation**을 구글에 치면 가장 먼저 나오는 것이 '선형 보간법(linear, bilinear, trilinear interpolation)'이 나온다.

우선 이 글에서는 linear interpolation(선형 보간법)만 다루겠다.

선형은 직선이란 뜻이다.
그렇다면 **보간법**이 무엇일까?
국립국어원 표준국어대사전을 보면 '(수학) 둘 이상의 변숫값에 대한 함숫값을 알고서, 그것들 사이의 임의의 변숫값에 대한 함숫값이나 그 근삿값을 구하는 방법 '을 뜻한다.

더 쉽게 보자면, 아래의 그림에서 p1과 p2 사이에 있는 점 p의 값을 추정하기 위해 '선형 보간법'을 사용할 수 있다.

<p style="text-align: center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/1D_linear_interpolation.jpg/300px-1D_linear_interpolation.jpg" alt="선형 보간법"></p>

<p style="font-size: 10px; text-align: center">https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95_%EB%B3%B4%EA%B0%84%EB%B2%95</p>

해당 그림에서 점 p를 구하는 공식은 아래와 같다.

\$$\frac{( (d2 * f(p1)) + (d1 * f(p2) )}{d1+ d2} = f(p)$

<p style="font-size: 14px;">*d1은 p에서 p1까지의 거리를, d2는 p에서 p2까지의 거리를 말한다. </p>

## 언제 interpolation을 사용하는가?

이렇게만 보면 언제 사용되는지 잘 모를 수 있다.
우리가 interpolation을 가장 많이 체감할 수 있는건 사진이다.

<p style="text-align: center"><img src="https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQ_h40jRZLx2_G3C3gyVRk5SOGbMBQjH8v3NCPuFCd6n_l91yyo&usqp=CAU" alt="많이 쓴 짤"></p>

위 짤처럼, 사람들이 퍼가다보면 사진이 점점 흐려지고 작아져서 확대해서 볼 수 없다. 이때 우리는 주로 '픽셀이 깨졌다'라고 표현한다.

원본 이미지 확대해보면 px들이 촘촘하게 있는데, 확대할 수 있을 때까지 해보면 이 px들 사이에 공백이 존재한다.

<img src="http://styleguide.co.kr/content/resolution-grid/responsive-grid-imgs/grid_ex12.png" alt="px">
<p style="font-size: 10px; text-align: center">http://styleguide.co.kr/content/resolution-grid/ratio-design.php</p>

이 공백을 바로 주변의 px값으로 채워주는데 이게 바로 interpolation이다.
주변에 있는 색상들로 공백인 px을 채워주는 것이다.

혹은 그라데이션도 interpolation으로 그릴 수 있다.

<p style="text-align: center; height: 300px;"><img src="https://i.pinimg.com/originals/ee/30/98/ee3098a219f2504df45b663e38e0f12a.png" alt="그라데이션"></p>
<p style="font-size: 10px; text-align: center">https://www.pinterest.co.kr/pin/760475087054803380/</p>

우리는 맨 위에 있는 분홍색 rgb와 아래 있는 하늘색 rbg를 정확하게 안다면, interpolation이 알아서 중간 값을 찾아서 그려준다.

## react-spring의 interpolation

사실 애니메이션은 시작 값을 x로, 끝나는 값을 y로 잡아서 for문으로 x값을 1씩 증가하여 y에 맞닿았을 때 종료해도 된다.

<iframe
     src="https://codesandbox.io/embed/move-circle-1r8ui?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="move circle"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-autoplay allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

https://www.w3schools.com/graphics/game_controllers.asp

하지만 움직이는 것이 매우 부자연스럽다. 이는 등속운동(균등한 속도로 움직이는 것)처럼 보여서 그런 것이다.
자연스러운 애니메이션을 그리고 싶다면, 처음 속도를 빠르게 설정하고, 점차 느리게 움직여야 된다.

<p style="text-align: center;"><img src="https://miro.medium.com/max/600/1*ADx1MvDi8Gl8yjnfWlUnFg.png" alt="ease"></p>
<p style="font-size: 10px; text-align: center">https://medium.com/motion-in-interaction/animation-principles-in-ui-design-understanding-easing-bea05243fe3</p>
