---
title: "[Javascript] Function(1)"
date: 2017-11-05 10:10:00
categories:
- Javascript
tags:
- Javascript
---
**Function은 명령어의 집합이다. 함수를 선언하는 방법은 두가지가 있어~~**
<br>

# 함수의 선언(function declaration)=> 선언적
```javascript
function add(a,b){
  return a+b;
}
var result=add(3,5);//함수의 호출
```
# 함수 표현식(function expression)=> 익명
```javascript
var add=function(a,b){//이름이 없는 익명함수, 이름으로 뭘 적어도 되지만 대부분 생략
  return a+b;
  var result=add(3,5);//함수의 호출
}
```

<br>

```javascript
var add=function(x,y){ //익명적 함수
  var result=x+y;
  return result;
}
console.log(add(3,5));//8
==============================
function f(){

return function add(x,y){return x+y;};
  }
console.log(f()(3,5))
```
자바스크립트에 함수는 항상 반환 값이 있다. return을 정해주지 않으면 undefined값이 반환된다.

```javascript
function add(x,y){

  result=x+y;

  console.log("length: "+length);
  console.log("arguments.length: "+arguments.length);
    for(i=0;i<arguments.length;i++){
      console.log(arguments[i]);
    }
  return result;
}
add(3,4,5,6)
```
# 함수의 호이스팅(Hosting)
```javascript
logMessage();   // 함수 호출
sumNumbers();  // 함수 호출

function logMessage() {            // 1. 함수 선언
  return 'worked';
}

var sumNumbers = function () { // 2. 함수 표현식
  return 10 + 20;
};
```

- 만일 위와 같은 문장이 있다면, 호이스팅에 의해 아래와 같이 해석된다.
(호이스팅은 선언부를 위로 옮기는 것을 말한다.)

```javascript
function logMessage() {
   return 'worked';
}
var sumNumbers; //undefined

logMessage();  // OK.
sumNumbers(); // 에러. Uncaught TypeError: sumNumbers is not a function

sumNumbers = function () {   //  var sumNumbers = function () { 에서 선언부가 분리됨.
  return 10 + 20;
};  //Number
```

함수 선언위치는 함수 호출과 관계가 없다. 함수표현식은 함수가 호출되기 이전에 위치해야한다.

# 함수호출시 입력된 인자(argument)를 저장하는 객체(배열 아니다!!!!)
`arguments`를 이용하여 파라미터의 갯수를 알며 접근할 수 있다.
```javascript
function findMax(a, b) {
     var i;
     var max = -Infinity   // 음의 무한대
     console.log(arguments);  

     for (i = 0; i < arguments.length; i++) {   //
           if (arguments[i] > max) {
               max = arguments[i];       // 배열처럼 접근 하지만 배열은 아니다!!!
           }
     }
     return max;
}
var max =  findMax(5,3,6,1,7);  // max = 7

console.log("findMax.length="+findMax.length);  // findMax.length는 함수 findMax에 선언된 매개변수의 개수
```

# 매개변수의 초기화와 유효성검사
```javascript
fucntion add(a, b) {
    b = b || 10;  // a의 값이 false값(0, undefined)인 경우 10으로 초기화한다.

    return a + b;
}
```

* 유효성 검사
null은 object를 위한 것이고, undefined는 변수, 속성 메서드를 위한 것

```javascript
if(myObj!==null&&typeof myObj!==="undefined")//에러
if (typeof myObj !== "undefined" && myObj !== null)   // OK
```
>일단 정의된 것인지부터 확인해야 한다. 정의되지 않았다면  `myObj!==null`에서 에러 발생한다.

-매개변수가 숫자인지 확인하기
```javascript
if(typeof x !== 'number') return '숫자를 전달하세요.';    // x의 값이 2이면 참,  '2'이면 거짓
```

**Hoisting - 모든 선언문을 소스코드의 맨위로 이동시키는 것, 가능하면 알아서 이동시켜라**

```javascript
x = 5; // 변수 x에 5를 저장. 변수의 선언은 아직 안함.
elem = document.getElementById("demo");  // Find an element
elem.innerHTML = x;                       // Display x in the element
var x; // 변수 x의 선언 - 아무런 문제가 없음. 선언문이 맨위로 자동이동될 것임.
```
그러나... 초기화된 선언문은 자동이동(Hoisting)되지 않는다. 선언은 이동하되, 초기화문장은 그 자리에 머물기 때문이다.(두 문장으로 쪼개짐)

```javascript
var x = 5; // Initialize x

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x + " " + y;           // y는 undefined

var y = 7; // Initialize y

위의 코드는 아래와 같이 호이스팅됨.

var x = 5; // Initialize x
var y;

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x + " " + y;           // y는 undefined

y = 7; // Initialize y
```

화살표 함수-ECMAScript6
그냥 람다식!!
