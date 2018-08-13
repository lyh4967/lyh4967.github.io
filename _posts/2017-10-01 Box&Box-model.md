---
layout:     post
title:      "[HTML&CSS] Box & Box model"
subtitle:   "박스에 대한 개념을 잡아보자"
date:       2017-10-01 10:02:00
author:     "Hoon"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
catalog:    true
tags:
    - HTML&CSS
---
## 레이아웃이 무너지는 이유가 뭘까나??
**박스에 대하 개념이 안잡혀있어!!!**
**이떻게 작동하는지만 알면 하산(下山)해라!!!~!~!**

보더박스: 테두리
패딩박스: 컨텐트와 보더 사이

# display속성
`block`,`inline`,`inline-block`,`flex`
<br>

## block
(1) 길막!!! 옆으로 오지못하게 길막을 한다!!
별도의 width값을 주지 않았을때 width는 100%가 되며 부모의 content-box의 width만큼 늘어난다.
<br>
(2)자식 박스가 width가 고정될경우 옆에 마진이 자동으로 생겨 길막형성!!, margin-left||right를 이용하여 좌우 배치를 할수도 있겠다~~~.
<br>
(3)별도의 height값이 주지 않았을때 자식의 크기 만큼  높이를 가진다. (margin)도 포함해서 계산된다.
<br>

```html
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="parent">
        <divclass="child">CHILD</div>
    </div>
</body>
</html>

<!-- style.css-->
.parent{
    background-color:#ecedef;
}
.child{
    background-color:0066ff;
    height:50px;
}
```
크롬개발자 도구로 픽셀확인해보면 알 수 있다.

## inline-->흐름!!!!
In Line!! 선 안에서 흐르는 녀석들이 inline 박스!!
`<p>`는 block 그러나 그안의 text들은 inline이다.
블록:인라인=면:선
블록:인라인=영역:흐름
인라인은 넓이를 줄이면 알아서 밑으로 내려간다. 대표적인 인라인 태그는 `<span>`
-->패딩은 라인들에게 아무런 영향을 끼치지 않는다, margin-bottom도 안먹힌다. 왜냐하면 인라인요소이기 때문이다. (하지만 컨텐트에는 마진과 패딩이 적용된다.), height를 바꿔도 안통한다.
<br>

## inline-block-->짬뽕!!
인라인 흐름안에 블록이 들어있다. 길막의 성질은 없고, 흐름의 성질이 있다. 또한 패딩,마진 모두 사용 가능하다.
<br>

## flex 갇
추후에 따로 자세하게 설명들어갑니다.
<br>
<br>
**box-sizing:border-box**
박스 사이징 설정을 잡아주면 보더까지 퐘해서 width와 height를 보더까지 포함해서 적용한다.

```html
*{
box-sizing:border-box;
}
```
