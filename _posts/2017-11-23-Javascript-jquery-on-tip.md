---
title: "[Javascript] jquery on메서드 팁"
date: 2017-11-23 10:10:00
categories:
- Javascript
tags:
- Javascript
---

**on()메서드는 현재존재하는 태그에만 이벤트를 연결한다**

```HTML
<body>
    <div id="wrap">
        <h1>Header</h1>
    </div>
</body>
<script>     
    $(document).ready(function(){
        $('h1').on('click',function(){//on 메서드는 현재 존재하는 태그에만 이벤트를 연결한다.
            var length=$('h1').length;
            var targetHTML=$(this).html();
            $('#wrap').append('<h1>'+length+'-'+targetHTML+'</h1>')
        })
    });
</script>
```
위의 경우 로드시에 생성된 h1태그에만 이벤트가 연결된다 따라서 이러한 경우에는 상위 태그에 이벤트를 연결하고 "h1 태그를 클릭했을때"를 검출해야한다

```HTML
<body>
    <div id="wrap">
        <h1>Header</h1>
    </div>
</body>
<script>     
    $(document).ready(function(){
        $('#wrap').on('click','h1',function(){//on 메서드는 현재 존재하는 태그에만 이벤트를 연결한다.
            var length=$('h1').length;
            var targetHTML=$(this).html();
            $('#wrap').append('<h1>'+length+'-'+targetHTML+'</h1>')
        })
    });
</script>
```

상위 개념이 애매한 태그는 document객체에 이벤트를 연결한다.
