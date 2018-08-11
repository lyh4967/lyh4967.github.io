---
title: "[Javascript] Javascript Summary"
date: 2017-10-30 10:10:00
categories:
- Javascript
tags:
- Javascript
- SUMMARY
---
**(1) ECMA Script5(2009)
(2) 출력과 입력
(3) 변수선언과 타입 typeof(var, let, const)
(4) 문자열 " 과 ' ==와 ===
(5) 조건식 30>20>10 ?**

# ECMAScript5
ECMAScript6에서는 템플릿 문자열이 추가되어 `text` 기호로 감싸 만들며 문자열 내부에 `${}`기호를 사용하고 내부에 표현식을 넣는다. 밑에서 언급할 let과 const도 추가된 부분 중 하나이다
```javascript
alert(`표현식 273+52의 값은 ${52+273} 입니다`);
```
<br>
# 출력과 입력
window.alert()-경고창
window.confirm()-확인창 출력 -->true, false
window.prompt("제목","내용")-입력한 내용을 문자열로 반환(아무것도 입력 안하면 null)
console.log()- 콘솔에 출력
window는 브라우저를  의미한다.
<br>

# 변수선언과 타입
숫자 : number (double)
문자 : string
논리: boolean ->true, false   
        3=true;  !3=false;  !!3=true (boolean으로 형변환)
undefined
 null 은 값이지 타입이 아니다!!
파생형:function, object, Object, Date. Array , ...
```javascript
console.log(!!NaN);-->"false"
console.log(!!0);-->"false"
console.log(!!null);-->"false"
console.log(!!undefined);-->"false"
var variable;
console.log(!!variable);-->"false"

var variableA=52;
let variableB=273;
const constantC=103;

console.log(variableA);
console.log(variableB);
console.log(constantC);
//전역스코프
{
  //스코프 A
  {
    //스코프 B
  }
}
```
| 키워드 | 구분 | 선언위치 | 재선언|
| :-------- | :-------- | :-------- |:-------|
| var | 변수 | 전역 스코프 | 가능 |
| let | 변수 | 해당 스코프 | 불가능 |
| const | 상수 | 상수 스코프 | 불가능 |
<br>

# 문자열 " 과 ' &  ==와 ====
```javascript
var x = "5" + 7; // x.valueOf() is 57,  typeof x is a string

"2" < "12" false // 사전순서로 12가 먼저니까... 2보다 작다.
"2" > "12" true  // 사전순서로 2가 나중이니까 크다.
1==“1”   true
0=="0"  true
```
==값비교
===값&타입 비교
```javascript
var str="abc";
var str2=new String("abc");
console.log(str==str2) //true
console.log(str===str2) // false
//==는 값만비교, ===는 타입까지비교

var str=new String("abc");
var str2=new String("abc");
console.log(str==str2) //false
console.log(str===str2) //false
//객체는 주소값을 비교하기때문에 false
```
