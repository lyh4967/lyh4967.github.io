---
layout:     post
title:      "[HTML&CSS] position float랑 position은 언제 써야할까?"
subtitle:   "What is a position"
date:       2017-10-07 10:02:00
author:     "Hoon"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
catalog:    true
tags:
    - HTML&CSS
---
**float는 영역 범위 내에서 촤르르~ 세워지는 느낌??? position은 완전히 여기저기 다닐 수 있는 느낌?? 포지션은 좌표로 움직이나~_~_~_~_~**

# 종류 && 기준점
## Position statc
-모든 요소의 기본값이다.
<br>

## Position Relative
-자신의 원래상태를 기억한다!!! 자신이 있던 곳을 기준으로 이동!!
<br>

## Position Absolute
-float랑 유사한 점이 많다. float가 집나간 자식이라면 absolute는 버린자식!!! 인라인 요소들이 인식을 못해요~!
그렇다면 absolute를 어떻게 사용할 것인가???
-기준점을 새로 정할 수 있다!! 부모가 relative or absolute or fixed라면 가준점으로 사용 가능하다.
<br>

## Position fixed
-viewport를 기준으로 배치되는 fixed. viewport(창에 보여지는 크기)를 기준으로 배치된다. 화면의 상단이나 하단에 고정시킬대 사용된다 (광고같은것들!!)
<br>
**Offset값 설정을 해줘야 적용이 된다.**
