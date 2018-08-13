---
title: "[JSP] JSP 정리해보자~!"
date: 2017-10-21 10:18:00
categories:
- JSP
tags:
- JSP
---
# 웹서버 요청방법(GET,POST)
GET: `<form>`,URL입력,`<a href="">`
POST: `<form>`
<br>

# request.getParameter("id")
`<form>`태그로 오는 데이터 혹은 URI로 넘어 오는 데이터를 받아 컨트롤러에서 사용한다.
<br>

# response.sendRedirect("~")
두번의 요청으로 원래 갖고있던 데이터는 없어지며 target URI로 이동한다.
RequestDispatcher(rd.forward(req,res)); 를 사용하면 1번의 요청으로 이동하며 데이터를 소유한 상태이다. 주로 필터적용시에 target URI를 넣고 해당페이지로 이동시에 사용한다.
<br>

# Servlet-->JSP
Servlet는 URL매핑과 데이터를 데이터를 요청하고 사용하며 내보낸다. Servlet에서 처리한 데이터를 JSP파일로 출력시킨다.
<br>

# 쿠키(브라우저에 저장) 세션(서버에 저장)
둘의 차이는 저장공간에 있다. 세션 또한 쿠키이며 서버쿠키라고도 불린다. 다음에 로그인시 아이디 저장과 같은 상대적으로 보안수준이 낮은 데이터 들은 쿠키에 담아 브라우저에 저장하고 아이디와 비밀번호를 이용한 로그인 상태 유지는 세션에 담아 서버에 저장한다. 쿠키와 세션 모두 수명을 정해 사용한다.
<br>

# JDBC
MVC모델과 DB를 Connect한다
(1)JDBC Driver
(2)DB URL
(3)User name
(4)User password
<br>

# 에러페이지
<br>

# <%page include>
navigation bar와 같이 페이지 전환 시에도 계속해서 출력시켜야하는 요소들은 따로 JSP파일로 만들어 여러 페이지 반복적으로 사용한다. <%page include>시 해당 JSP에 있는 코드들을 그대로 복해서 붙여넣기 하는것과 같은 효과가 있다.
<br>

# MVC(Model View Controller)
UserDao, Dao
Model: Controller과 View, DB가 주고받을 객체라 할 수 있다.
Controller: URI로 넘어오는 데이터와 JSP에서 넘어오는 데이터를 처리하고 다시 JSP로 데이터를 보낸다
View: Controller에서 보낸 데이터를 사용자의 기호에 맞게 출력시키는 역할을 한다.
<br>

# Listener & Filter
JDBCDriver 로딩 (로그인 체크)
web.xml파일 URI을 등록하고 해당경로로 요청이 들어올 시에 Controller에서 이를 감지해 URL에 접근을 막는다. 주로 권한이 필요한 페이지에 접근시 로그인페이지로 자동이동 시키거나 로그인시에 자동을 Driver을 연결하는 부분에 쓰인다.
<br>

# EL & JSTL
java와 html코드가 섞여 보기싫은 파일들은 조금더 html에 맞추어 가독성을 증가시키기위한 목적으로 만들어진 문법이다.
`${}`로 EL을 사용할 수 있으며 JSTL의 경우 해당 문법에 맞는 URI 라이브러리를 추가해 사용해야 한다.
ex) core태그 사용시에
<%@ taglib prefix="form" uri="http://java.sun.com/jsp/jstl/core">
를 추가해야 한다.
<br>

# validator, war 배로
validator => 유효성 검사
war => Deplay(배치)
<br>

# Filedownload files
