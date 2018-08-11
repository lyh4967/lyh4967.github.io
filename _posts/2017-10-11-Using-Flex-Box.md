---
title: "[HTML&CSS Flex] Box를사용합시다"
date: 2017-10-11 10:18:00
categories:
- HTML&CSS
tags:
- HTML6
---
**첫째, 나 플렉스 박스 쓸꺼야!**
**둘째, 방향은 어디로 정렬할 거야??**
**셋째, 크기 유지할꺼야?? 아니면  알아서 조정할꺼야??**
**넷째, 느낌적인 느낌대로 정렬!**
<br>

# 정렬할 요소를 가진 부모에게 display:flex만 주면 준비완료!
<br>

# 방향정렬 flex-direction:row||column(기본값은 row) 디렉션과 같은방향축은 main axis, 반대방향축은 cross axis
<br>

# 어떻게든 한줄을 유지할껀가 아니면 본래사이즈를 유지할 껀가.
display:flex; flex-wrap:nowrap --> 박스를 알아서줄여 한줄로 만든다. (기본값임)
display:flex; flex-wrap:wrap --> 공간이 모자라면 맡으로한칸 떨어지게!!
<br>

# main axis를 기준으로 정렬 => justify-content, cross axis를 기준으로 정렬하려면 align-items. --> 직접하며 느낌을 익혀라!!!
<br>

# 자손들의 block 변경
.child:nth-child(1){order:3}
.child:nth-child(2){order:1}
.child:nth-child(3){order:2}
자손 블락들의 순서가 변경된다.
<br>

**정렬!, 어렵지 않아요~~ flexboxfroggy로 연습해보아요~**
[flexboxfroggy 사이트](http://flexboxfroggy.com/)
