---
layout:     post
title: "[HTML&CSS] 반응형 웹사이트 어렵지 않아요!!"
subtitle:   "Responsive Web"
date: 2017-10-14 10:18:00
categories:
- HTML&CSS
tags:
- HTML&CSS
---
**반응형 웹사이트:PC뿐만아니라모바일에서도 최적화된 CSS를 볼 수 있게 해주는 반응 형 웹사이트**
<br>
 **꼭 필요한3가지!!**
 **(1) Viewport Meta tag**
 **(2) Media Query**
 **(3) Grid System**

# Viewport Meta tag
head요소 내부에 Viewport Meta Tag를 적어야한다..!!
```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```
a. width=device-width => viewport width를 디바이스 width와 맞춘다
b. user-scalable => 유저가 크기조정을 할 수 없다.
c. initial-scale=1.0 => 시작 배율은 1.0x로 정한다.
d. maximum-scale=1.0 => 최대 배율을 1.0x로 정한다.
e. minimum-scale=1.0 => 최소 배율을 1.0x로 정한다.
<br>

# Media Query는 if/else와 똑같다.!!
```html
@media screen and(min-width:500px) and(max-width:759px){
    body{
        background-color: #2c3e50;
    }
}
```
미디어 관련해서 설정을 해줄껀데 미디어 중에서도 스크린에 대해서 설명할꺼야!! min width:500px이고 759px일때 배경색을 변경할꺼야!! (500<width<759px)
