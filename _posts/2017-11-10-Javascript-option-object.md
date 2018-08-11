---
title: "[Javascript] 옵션 객체"
date: 2017-11-10 10:10:00
categories:
- Javascript
tags:
- Javascript
---
**옵션객체란? 함수의 매개변수로 전달하는 객체를 '옵션 객체'라고 부른다**

```javascript
function test(options){
  options.valueA=options.valueA||10;
  options.valueB=options.valueB||20;
  options.valueC=options.valueC||30;

  alert(options.valueA+':'+options.valueB+':'+options.valueC);
}
test({
  valueA:52,
  valueB:273
});      //52:273:30 출력
```
함수의 파라미터로 객체를 전달받았다
초기화하는  코드는 굉장히 자주 나오는 익숙해지자!

# 깊은 복사 참조 복사
참조복사(얕은복사)는 객체가 저장된 메모리의 주소를 저장한다. 따라서 복사된 새로운 객체도 기존객체의 메모리를 참조하기 때문에 아래와 같은 결과가 출력된다.
```JavaScript
var originalArray=[1,2,3,4];
var newArray=originalArray;

originalArray[0]=273;
alert(`${originalArray}::::::${newArray}`); //273,2,3,4::::::::273,2,3,4
```
깊은복사는 키와 값을 하나하나 옮겨야한다.
```javascript
function clone(obj){
  var output={};
  for(var i in obj){
    output[i]=obj[i];
  }
  return output;
}
var original={a:10,b:20};
var referenced=original;
var cloned=clone(original);

original.a=20;
alert(JSON.stringify(referenced,null,2)) //a:20 ,b:20
alert(JSON.stringify(cloned,null,2)) //a:10 , b:20
```

# 전개연산자로 배열복사 테크닉(ECMA Script6)
```javascript
//깊은 복사
const originalArray=[1,2,3,4,5];
const newArray=[...originalArray];
originalArray[0]=52;
originalArray[1]=273;
alert(originalArray); //52,273,3,4,5
alert(newArray); //1,2,3,4,5

//배열 병합
var originalArray=[1,2,3,4,5];
var newArray=[...originalArray];
originalArray[0]=52;
originalArray[1]=273;
alert(originalArray);
alert(newArray);
var newArraySum=[...originalArray,...newArray];
alert(newArray);
```
