---
title: "[Spring] RESTFul API 디자인(1)"
date: 2017-10-24 10:20:00
categories:
- Spring
tags:
- Spring
- RESTFul
---
**개발자들에게 중요한 자질중 하나는 독학능력이다....!!**
**반복적인 학습은 뇌의 과부하 없이 자연스럽게 스며든다. 책을 가볍고 빠르게 훑어준다.**
<br>

**REST방식~**
뭐하나 바뀔때마다 화면 깜빡깜빡여야 되고 아주 번거로워 죽겠어!! 요새 앵귤러나 리액트도 백단에서 돌아가 동적으로 처리해준다

기존에 해온 방식을 Http방식이라고 하면 REST방식은 Method가 여러가지 있다 (DELETE,PUT,PATCH 등등) SQL문으로 생각하면  DELETE=delete, (PUT,DELETE)=update

이러한 RESTful형식은 JSON형식의 데이터를 받건 보내기위해 많이 사용하는데 spring의 경우 JSON데이터를 쉽게 처리해주는 애너테이션 `@ResponseBody`, `@RequestBody`를 사용한다. 하지만 위의 두 애너테이션을 사용하기 위해서는 jackson-databind 의존성 주입이 필요하다.
![jackson](/image/2017-10-24/jackson.png)
하지만 클래스에 `@RestController`을 붙이면 위의 `@RequestBody` 혹은 `@ResponseBody` 없이도 사용이 가능하다. (실제로는 `@RequestBody`와 `@ResponseBody`가 적절한 위치에 붙으며 생략된다.)

하지만 RESTful형식은 view단까지 끌어가지 않는다. 따라서 개발시 `ResponseEntity`(JSON,Http상태)를 보낸 결과를 확인한다.
![ARC](/image/2017-10-24/JSON_ARC.PNG)
크롬 확장프로그램 Advanced REST client를 이용하여 제이슨 데이터통신상태를 확인할 수 있다.
```java
@RestController
class ReplyController{
public ResponseEntity<String> register(@RequestBody ReplyVO vo){
		ResponseEntity<String> entity=null;
		try {
			service.addReply(vo);
			entity=new ResponseEntity<String>("SUCCESS",HttpStatus.OK);
		}catch(Exception e) {
			e.printStackTrace();
			entity=new ResponseEntity<String>(e.getMessage(),HttpStatus.BAD_REQUEST);
		}
		return entity;
	}
}
```
`@RequestBody`: JSON-->obj
`@ResponseBody`: obj-->JSON
클래스 위에 `@RestController` 을 붙임으로
```java
public @ResponseBody ResponseEntity<String>
```
위의 코드에서 `@ResponseBody`가 생략된다.

****
