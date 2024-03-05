---
title: "PID 제어란?"
date: 2024-03-05 20:13:00 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [pid, control, automation]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://assets-global.website-files.com/64ed06228d24e5d52132b49f/64edfa644cf457e21d628e17_64ecb5a1f70f5bb498e7aa4a_PID-Controller-Explained.png" 
    alt: 
---

# PID 제어
---

- 비례-적분-미분 제어기(**Proportional-Integral-Differential controller**)
	- 줄여서 PID 제어 라고 함
- 실제 응용분야에서 가장 많이 사용되는 대표적인 형태의 제어 기법
- 피드백 제어기의 형태를 가지고 있으며, 제어하고자 하는 대상의 출력값과 비교하여 오차를 게산하고 이 오차값을 이용하여 제어에 필요한 제어값을 계산하는 구조

![공식](https://wikimedia.org/api/rest_v1/media/math/render/svg/82e28e130699d4c7e795046f5b44b729a8e48e86)

# 1. 단순 On/OFF 제어
---

- 제어 조작량을 0%와 100%사이를 번갈아 가면서 제어함
- 조작량의 변화가 너무 크고, 실제 목표값에 대해 지나치게 반복하기 때문에 목표값 부근에서 요동친다

# 2. 비례 제어(P제어)
---

![proportional Control](https://i.ytimg.com/vi/E0rdLQLMZdA/maxresdefault.jpg)
- 가장 직관적으로 에러값이 비례해서 주는 제어법
- 조작량을 목표값과 현재위치와의 차이(오차)에 비례한 크기가 되도록 하며, 서서히 조절하는 제어방식
- 가장 구현하기 쉬운 방식이며, 빠른 시간 내에 원하는 에러를 줄여줌
- 오차가 줄어도, 오차에 비례해서 제어값을 주기 때문에 목표치를 초과하는 현상 - 오버슈팅(overshooting)이 발생함
- 혹은 조작량이 너무 작아 더 세밀하게 제어할 수 없을때, 목표치에 매우 가까운 상태에서 안정되버리는 현상이 발생함

# 3. 적분 제어(PI제어)
---

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/afb71d64-d9df-4c1e-a548-7acdff82f337)

- 비례 제어의 단점을 보완하기 위한 제어 방법
- 오차의 적분값에 비례해서 제어값을 주어 잔류 에러를 줄이는 방법
- 목표값이 일정하다면 효과적이지만, 값의 변화가 생기면 적분의 게인값이 늘어 시스템의 발산을 유발하기 쉬움
- 또한, 목표값으로 제어하기 위해 일정시간이 필요함
- 이러한 발산을 막기 위해 적분 게인은 anti-wind up 기법들을 사용하기도 함

# 4. 비례-적분-미분 제어(PID 제어)
---

![pidcontroll](https://upload.wikimedia.org/wikipedia/commons/3/33/PID_Compensation_Animated.gif)
- 오차값을 미분하여 제어신호를 만들어 내는 미분제어를 비례제어에 병렬로 연결하여 사용하는 제어기법
- 급격히 일어나는 외부의 환경에 대해 편차를 보고, 이전 오차와의 차가 큰 경우에는 조작량을 많이 하여 기민하게 반응하도록 한다.
- 미분 동작은 이번 오차와 이전 오차를 비교하여 편차의 대소 크기에 의해 조작량을 기민하게 반응하도록 하는 동작




참조
[PID 제어기 - Wiki](https://ko.wikipedia.org/wiki/PID_%EC%A0%9C%EC%96%B4%EA%B8%B0)
[PID제어란!!](https://blog.naver.com/sppe12/110085291875)
[고전 제어의 절대강자 PID Control(제어) A to Z - 1편](https://hyein-robotics.tistory.com/entry/%EA%B3%A0%EC%A0%84-%EC%A0%9C%EC%96%B4%EC%9D%98-%EC%A0%88%EB%8C%80-%EA%B0%95%EC%9E%90-PID-Control%EC%A0%9C%EC%96%B4-A-to-Z-1-%ED%8E%B8)
[60. PID 제어 란?(비례/적분/미분)](https://m.blog.naver.com/jsrhim516/222015965919)
