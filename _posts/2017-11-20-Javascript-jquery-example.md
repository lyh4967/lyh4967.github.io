---
title: "[Javascript] jquery 활용예제"
date: 2017-11-20 10:10:00
categories:
- Javascript
- Example
tags:
- Javascript
---

**jquery 축구,배구 ~~ 활용예제**

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="jquery-3.2.1.js"></script>
</head>
<body>
    <div>
        <button class='checkAll'>전체 선택</button>
        <button class='uncheckAll'>전체 해제</button>
    </div>
    <div>
        <label for="soccer"><input type="checkbox" value='soccer'>축구</label>
        <label for="baseball"><input type="checkbox" value='baseball'>야구</label>
        <label for="basketball"><input type="checkbox" value='basketball'>농구</label>
        <label for="volleyball"><input type="checkbox" value='volleyball'>배구</label>
        <label for="tabletennis"><input type="checkbox" value='tabletennis'>탁구</label>
        <label for="hocky"><input type="checkbox" value='hocky'>하키</label>
    </div>
    <textarea name="" id="" cols="30" rows="10"></textarea>
</body>
<script>
    $(document).ready(function(){    
        var str=[];
         $('.checkAll').on('click',function(){//전체 선택
             $('input[type=checkbox]').each(function(){
                 if(!this.checked){
                     this.checked=true;
                     str.push($(this).val());
                     $('textarea').text(str);
                 }               
             })
        });
        $('.uncheckAll').on('click',function(){//전체 해제
            $('input[type=checkbox]').each(function(){
                if(this.checked){
                 this.checked=false;
                 for(let i=0;i<str.length;i++){
                    if(str[i]==$(this).val())
                    str.splice(i,1);
                }
                 $('textarea').text(str);
                }
             })
        });
        $('input[type=checkbox]').on('click',function(index){//각자 선택
            if(this.checked){
                str.push($(this).val());
            }else{
                for(let i=0;i<str.length;i++){
                    if(str[i]==$(this).val())
                    str.splice(i,1);
                }             
            };         
            $('textarea').text(str);
        });
    })
</script>
</html>
```
