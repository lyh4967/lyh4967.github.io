---
title: "[Javascript] 생성자 함수"
date: 2017-11-11 10:10:00
categories:
- Javascript
tags:
- Javascript
---
**자바스크립트에서의 생성자를 알아보자**

```JavaScript
function Student(name,korean,math,english,science){
  //속성
    this.이름=name;
    this.국어=korean;
    this.수학=math;
    this.영어=english;
    this.과학=science;
      //메서드
   // this.getSum=function(){
    //return this.국어+this.수학+this.영어+this.과학;
    //};
    //this.getAverage=function(){
     //  return this.getSum()/4;
    //};
    //this.toString=function(){
    //return this.이름+'\t'+this.getSum()+'\t'+this.getAverage();
    //};
}
Student.prototype.getSum=function(){
   return this.국어+this.수학+this.영어+this.과학;
}
Student.prototype.getAverage=function(){
  return this.getSum()/4;
}
Student.prototype.toString=function(){
  return this.이름+'\t'+this.getSum()+'\t'+this.getAverage();
}
var students=[];
students.push(new Student('윤하린',96,98,92,98));


students.push(new Student('윤인아',96,92,98,23));
var output='이름\t총점\t평균\n';
for(var i in students){
  output+=students[i].toString()+'\n';
}
alert(output)

var student=new Student('영훈이',42,78,67,87);
alert(student instanceof Student);//true
alert(student instanceof String);//false


```
