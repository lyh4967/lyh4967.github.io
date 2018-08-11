---
title: "[Javascript] 사천성 만들기"
date: 2017-11-16 10:10:00
categories:
- Javascript
- Example
tags:
- Javascript
---
**사천성 짝맞추기 게임**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
   <div>
    <img id='c0_1' src="resource/resource/0_1.jpg" alt="">
    <img id='c0_2' src="resource/resource/0_2.jpg" alt="">
    <img id='c1_1' src="resource/resource/1_1.jpg" alt="">
    <img id='c1_2' src="resource/resource/1_2.jpg" alt="">
    <img id='c2_1' src="resource/resource/2_1.jpg" alt="">
    <br>
    <img id='c2_2' src="resource/resource/2_2.jpg" alt="">
    <img id='c3_1' src="resource/resource/3_1.jpg" alt="">
    <img id='c3_2' src="resource/resource/3_2.jpg" alt="">
    <img id='c4_1' src="resource/resource/4_1.jpg" alt="">
    <img id='c4_2' src="resource/resource/4_2.jpg" alt="">
    <br>
    <img id='c5_1' src="resource/resource/5_1.jpg" alt="">
    <img id='c5_2' src="resource/resource/5_2.jpg" alt="">
    <img id='c6_1' src="resource/resource/6_1.jpg" alt="">
    <img id='c6_2' src="resource/resource/6_2.jpg" alt="">
    <img id='c7_1' src="resource/resource/7_1.jpg" alt="">
    <br>
    <img id='c7_2' src="resource/resource/7_2.jpg" alt="">
    <img id='c8_1' src="resource/resource/8_1.jpg" alt="">
    <img id='c8_2' src="resource/resource/8_2.jpg" alt="">
    <img id='c9_1' src="resource/resource/9_1.jpg" alt="">
    <img id='c9_2' src="resource/resource/9_2.jpg" alt="">
    </div>
    <div>
    <button id='turnAll'>뒤집기</button>
    <button id='hide'>숨기기</button>
    <button id='show'>보이기</button>
    <button id='shuffle'>섞기</button>

    </div>
</body>
</html>
```
```Javascript
<script>
    window.onload=function(){
        //변수선언
        var isTurnAll=true;
         checkArr=[];
        var cnt=0;
        var checkFlag=true;//setTimeout함수 실행전까지 클릭x

        var turnAll=document.getElementById('turnAll');
        var hide=document.getElementById('hide');
        var show=document.getElementById('show');
        var shuffle=document.getElementById('shuffle');

        //카드 2차원배열 선언
        var cardArr=new Array(10);
        for(let i=0;i<10;i++){
            cardArr[i]=new Array(2);
        };

        //경로 2차원배열 선언
        var srcArr=new Array(10);
        for(let i=0;i<10;i++){
            srcArr[i]=new Array(2);
        };

        //배열에 카드 넣기, 경로 배열에 넣기
        for(let i=0;i<10;i++){
            for(let j=0;j<2;j++){
                cardArr[i][j]=document.getElementById('c'+i+'_'+(j+1));
                srcArr[i][j]="resource/resource/"+i+"_"+(j+1)+".jpg";
            };
        };
       /* //경로 배열에 넣기
        for(let i=0;i<10;i++){
            for(let j=0;j<1;j++){
                srcArr[i][j]="resource/resource/"+i+"_"+j+".jpg";
            }
        }*/

        //뒤집기
        turnAll.onclick=function(){
            if(isTurnAll){
                for(let i=0;i<cardArr.length;i++){
                    for(let j=0;j<cardArr[i].length;j++){
                            cardArr[i][j].src="resource/resource/back.jpg";
                    };
                };
                isTurnAll=false;
            }else{//srcArr의 경로를 cardArr에 넣는다.
                for(let i=0;i<cardArr.length;i++){
                    for(let j=0;j<cardArr[i].length;j++){
                            cardArr[i][j].src=srcArr[i][j];
                    };
                };
                isTurnAll=true;
            };
        };
        //숨기기
        hide.onclick=function(){
            for(let i=0;i<cardArr.length;i++){
                for(let j=0;j<cardArr[i].length;j++){
                        cardArr[i][j].style.visibility='hidden';
                    };
                };
        };
        //보이기
        show.onclick=function(){
             for(let i=0;i<cardArr.length;i++){
                for(let j=0;j<cardArr[i].length;j++){
                        cardArr[i][j].style.visibility='visible';
                 };
            };
        };
        //섞기
        shuffle.onclick=function(){
            //경로섞기
            for(let i=0;i<srcArr.length;i++){
                for(let j=0;j<srcArr[i].length;j++){
                    let k=parseInt(Math.random()*srcArr.length);
                    let l=parseInt(Math.random()*srcArr[0].length);
                    let temp=srcArr[i][j];
                    srcArr[i][j]=srcArr[k][l];
                    srcArr[k][l]=temp;
                 };
            };
            //cardArr에 srcArr대입
            for(let i=0;i<cardArr.length;i++){
                for(let j=0;j<cardArr[i].length;j++){
                    cardArr[i][j].src=srcArr[i][j];
                    cardArr[i][j].alt='';
                 };
            };
        };
 //=============================짝맞추기================================


        for(let i=0;i<cardArr.length;i++){
            for(let j=0;j<cardArr[i].length;j++){
                cardArr[i][j].onclick=function(){
                    if(checkFlag){
                        if(cardArr[i][j].alt!='front'){//앞면이면 찍지 못하게한다.
                            cardArr[i][j].src=srcArr[i][j];
                            checkArr.push(cardArr[i][j]);
                            cardArr[i][j].alt='front';//뒤집음 속성 할당
                            cnt++;
                            console.log(cnt);
                            if(cnt==2){
                                checkFlag=false;
                                setTimeout(checkFn,500);
                                cnt=0;
                            }
                        }
                    }
                }
            }
        }

        //카드 비교함수
        var checkFn=function (){
            console.log(checkArr.length);
             console.log(checkArr[0].src.charAt(41));
            console.log(checkArr[1].src.charAt(41));
            var checkcard1=checkArr[0];
            var checkcard2=checkArr[1];
            if(checkArr.length==2){//숫자가 같은지, 그리고 같은카드를 눌렀는지 확인
                if((checkcard1.src.charAt(41)==checkcard2.src.charAt(41))&&(checkcard1.id!=checkcard2.id)){
                    checkArr=[];
                    checkFlag=true;
                    return;
                }else{
                    checkcard1.src="resource/resource/back.jpg";
                    checkcard2.src="resource/resource/back.jpg";
                    checkcard1.alt='';
                    checkcard2.alt='';
                    checkArr=[];
                }
            }checkFlag=true;
        }
     }

</script>
```
