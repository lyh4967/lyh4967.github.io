---
title: "[Javascript] 클로저"
date: 2017-11-07 10:10:00
categories:
- Javascript
tags:
- Javascript
---
**함수를 리턴하는 함수를 사용하는 가장 큰 이유는 클로저(closure)때문이다**

```javascript
function test(name){
   output='Hello'+name+'..!';
  return function(){
    alert(output);
  }
}
test('JavaScript')();
```
output은 지역변수로 함수가 종료될 때 사라져야한다. 하지만 이후에도 활용될 가능성이있어 자바스크립트는 변수를 제거하지 않고 남겨둔다.

따라서 test()함수로 생성된 공간 혹은 리턴된 함수 혹은 지역변수output를 클로저라고 부른다. 대게 본함수를 감싼 테두리 함수를 클로져라 칭한다.

**반복문과 콜백 함수문제**
```JavaScript
for(var i=0;i<3;i++){
  setTimeout(function(){
    alert(i);
  },0);
}//3을 3번 출력

for(var i=0;i<3;i++){
  (function(closed_i){
    setTimeout(function(){
      alert(closed_i);
    },0);})(i);
}//0, 1, 2 출력
```
`setTimeout()`가 호출되는 시점은 반복문이 모두 끝난시점이다. 따라서 클로저를 이용하여 `closed_i`에값을 저장할 수있게한다.
