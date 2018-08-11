---
title: "[Javascript] 객체"
date: 2017-11-19 10:10:00
categories:
- Javascript
tags:
- Javascript
---
**In JavaScript, the thing called this, is the object that "owns" the JavaScript code.**

자바스크립트에서는 this라고 불리는 것은 해당 코드를 소유하는 객체를 의미한다.

# 전역 범위에서 `this`는 `window`(브라우저)를 가리킨다.
<br>

# `new` 연산자와 함께 사용된 경우 `this`는 객체 자신을 가리킨다
```javascript
function Person(name, age) {
   this.name = name;  // 객체자신(this)에 name이라는 속성을 추가
   this.age  = age;
}

var p = new Person("asdf", 10);

console.log(p.name);
console.log(p.age);
```
<br>

# `new`연산자와 함께 사용되지 않는 경우 `this`는 전역객체(`window`)를 가리킨다
```javascript
function Person(name, age) {
 this.name = name;    // 전역객체(this)에 name이라는 속성을 추가
 this.age  = age;
}
var p = Person("asdf", 10);

console.log(window.name);   // asdf
console.log(window.age);     // 10

console.log(p.name);   // 에러 undefined
console.log(p.age);      // 에러 undefined
```
<br>
**[주의] 아래와 같이 new 연산자 없이 객체를 생성한경우에는 this가 객체자신을 가리킴**

```javascript
var myObject = {
   firstName:"John",
   lastName: "Doe",
   fullName: function () {
       return this    // 객체 자신을 나타냄
   }
}

var obj = myObject.fullName();
console.log("obj.firstName="+obj.firstName);
```
<br>

# HTML에서의 this
```HTML
<h1 onclick="this.innerHTML='Ooops!'">Click on this text!</h1>  // this는 <h1>을 가리킨다.
<h1 onclick="changeText(this)">Click on this text!</h1>   // this는 <h1>을 가리킴
<span onmouseover="this.style.color='red'" onmouseout="this.style.color='black'">Mouse over me!</span>
```

# 중첩된 함수 내에서의 this
```javascript
var myObj = {
    func1 : function() {
        console.log(this); // myObj
        var func2 = function() {
             console.log(this); // window
              var func3 = function() {
                    console.log(this); // window
              }();
        }();
    }
}
```
중첩된 함수 내에서도 this를 사용하려면.. 별도의 변수에 this를 저장하자.
```javascript
var myObj = {
    myProp : 'I can see the light',
    myFunc : function(){
         var that = this;  // this의 값을 that에 저장
         var myFunc2 = function() {
             console.log(that.myProp);  // that은 myObj
//          console.log(this);   // this는 window
         }();
    }
}
myObj.myFunc();
```
