---
title: "[Javascript] 문서객체 가져오기"
date: 2017-11-14 10:10:00
categories:
- Javascript
tags:
- Javascript
---
**문서 객체 배열에 for in 반복문을 사용하면 안된다. for in 반복문을 사용하면 안되는 이유는 문서 객체이외의 속성에도 접근하기 때문이다.**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>INDEX</title>
</head>
<body>
    <h1 id="header-1">Header</h1>
    <h1 id="header-2">Header</h1>
</body>
<script>
 window.onload=function(){
 var headers=document.getElementsByTagName('h1');
 var output='';
 for(let i=0;i<headers.length;i++){
            output+=i+'\n'
        }
        alert(output);   //0,1출력
   };
  for(var i in headers){
           output+=i+'\n';
       };
       alert(output) //0
                     //1
                     //header-1
                     //header-2
                     //length
                     //item
                     //namedItem
}
</script>
```
속성 자체에 접근하기 때문에 꼭!!!! 단순 for 반복문을 사용해야한다.!!
getElementsByName()메서드 또한 동일하다.
