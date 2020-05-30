---
title: 'react-spring: Hooks api'
date: 2020-5-31 18:21:13
category: 'react-spring'
draft: true
---

## Hooks api

Hooks api는 크게 5가지로 나뉜다.

- **useSpring**: 하나의 spring의 데이터가 from: a => to: b로 움직이는 것
- **useSprings**: 여러개의 springs, 각 데이터가 자신만의 상태로 움직이는 것 (dependency 존재)
- **useTrail**: 여러개의 springs가 하나의 데이터셋처럼 이전의 spring을 따라 움직이는 것 (dependency가 없음)
- **useTransition**: list의 아이템들이 추가/제거/업데이트 될 때 animation 발생
- **useChain**: 다수의 animation을 묶어서 연쇄적으로 발생

### 1. useSpring

<iframe
     src="https://codesandbox.io/embed/q3ypxr5yp4?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="basic useTransition masonry grid"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-autoplay allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

<br>

<p style="font-size: 13px; font-style: italic">오역이 있을 수 있습니다. 피드백은 언제나 환영합니다!</p>
