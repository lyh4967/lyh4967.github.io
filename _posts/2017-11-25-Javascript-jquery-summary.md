---
title: "[Javascript] jquery 요약"
date: 2017-11-25 10:10:00
categories:
- Javascript
tags:
- Javascript
- SUMMARY
---
**$(선택자).검색&탐색,작업(); 이벤트 처리**

# 선택자
 `*` : wildcard 전체선택
 `.` : class
 `h1`: 태그선택
 `h1, p`: h1과 p둘다
 `h1 p`: h1안에 후손 p
 `h1>p`: h1의 바로아래 자손p
 `h1~p`: h1의 형제p (like 어깨동무~) sibling
 `h1+p`: h1의 형제중 바로옆에 p
 [w3school 선택자 정리](https://www.w3schools.com/jquery/trysel.asp)

 `$('div h1')->$('h1','div')`
 `$('div this')->$('h1',this)` // 자바스크립트 508page 참조
 <br>

```javascript
$('#wrap').on('click','h1',function(){
   $(this).html()    //this=>h1
})
```

# 검색과 탐색
`filter()`:
`find()`:
`eq()`:
`first()`:
`last()`:
`not()`:
`has()`:
`is()`:
`add()`:

# 작업
`addClass()`: 선택자에 class를 추가한다.
`removeClass()`: 선택자에 class를 제거한다.
위 두개 메서드는 `toggle()`로 사용가능하다.
`attr()`: 선택자에 속성을 추가한다.
`removeAttr()`: 선택자에 속성을 제거한다.
위 두 메서드보다는 `prop()`를 더 많이 쓴다.
`html()`: html로 출력시킨다.
`text()`:
`val()`: 해당 선택자의 value값을 받아온다.
`each()`: 선택자가 여러개 존재시에 각 선택자 객체를 차례로 받아온다.

# 생성, 추가, 삭제
`$()`: 선택자의 내용을 생성한다.
`remove()`: ?삭제?
`clone()`: 선택자의 내용을 복제한다.
`empty()`: 선택자의 내용을 지운다.
`append()`: 해당 선택자의 뒤에 내용을 추가한다. ->`appendTo()`
`prepend()`: 해당 선택자의 앞에 내용을 추가한다. -> `prependTo()`
`after()`: ?다시공부?->`insertBefore()`
`before()`: ?다시공부?->`insertAfter`

# 이벤트 처리
`on()`:
`off()`:
`one()`:
`trigger()`:
`scroll()`:
`keypress()`:
`ready()`:
