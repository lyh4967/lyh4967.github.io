---
title: "[Spring] AOP, Transaction, Interceptor"
date: 2017-10-27 10:10:00
categories:
- Spring
tags:
- Spring
- AOP
---
# AOP
**Aspect: 포인트컷(어디에서) + 어드바이스(무엇을 할 것인지)**

**Advice: 각 조인포인트에 삽입되어져 동작할 수 있는 코드**
**Join points: 횡단 관심 모듈의 기능이 삽입되어 동작할 수 있는 실행 가능한 특정위치**
**Pointcuts: 어떤 클래스의 어느 조인포인트를 사용할 것인지를 결정하는 선택기능**
**Weaving: 포인트컷에 의해서 결정된 조인 포인트에 지정된 어드바이스를 삽입하는 과정**

target객체를 호출하면 실제 객체를 감싸고 있는 proxy를 통해서 호출이 전달된다. 로직적으로 필요한 부분은 아니지만 개발하는 입장에선 필수적인 부분이다

```java
@Before("execution(* org.zerock.service.MessageService*.*(..))")
	public void startLog(JoinPoint jp) {
		logger.info("--------------------");
		logger.info("--------------------");
	}
@Around("execution(* org.zerock.service.MessageService*.*(..))")
	public Object timeLog(ProceedingJoinPoint pjp) throws Throwable{
		long startTime=System.currentTimeMillis();
		logger.info(Arrays.toString(pjp.getArgs()));

		Object result=pjp.proceed();

		long endTime=System.currentTimeMillis();
		logger.info(pjp.getSignature().getName()+" : "+(endTime-startTime));
		logger.info("===================================");

		return result;
	}
```
* `@Before` => 메서드 실행 전에 실행한다.
* `@Around` => 코드 안에 jp를 넣고 jp앞뒤로 코드를 실행한다.
주로 메서드를 실행하는데 걸리는 시간혹은 메서드의 시작을 log에 표시하기 위해 사용된다.
<br>
### Transaction
A와 B로 구성되어 있는경우 A 메서드와 B메서드를 하나로 묶어  둘중 하나라도 에러가 나면 rollback되어야 한다. 개발자 입장에서 상당한 편의를 제공해주는 기능이다.

### Interceptior 로그인처리
Filter와 기능적으로 동일하다. 동작방식은 AOP와 유사하며 가장 중요한 차이는 `HttpServletRequest`, `HttpServletResponse`를 사용함에 있으며 `HandlerInterceptor`인터페이스를 구현해서 사용이 가능하다.
~~~java
@Override
	public void postHandle(HttpServletRequest request,HttpServletResponse response,Object handler,ModelAndView modelAndView) throws  Exception{
		HttpSession session=request.getSession();
		ModelMap modelMap=modelAndView.getModelMap();
		Object userVO=modelMap.get("userVO");
		if(userVO !=null) {
			logger.info("new login success");
			session.setAttribute(LOGIN, userVO);
			//response.sendRedirect("/");
			if(request.getParameter("useCookie")!=null) {
				logger.info("remember me.....");
				Cookie loginCookie=new Cookie("loginCookie",session.getId());
				loginCookie.setPath("/");
				loginCookie.setMaxAge(60*60*24*7);
				response.addCookie(loginCookie);
			}
			Object dest=session.getAttribute("dest");
			response.sendRedirect(dest!=null?(String)dest:"/");
		}
	}

	@Override
	public boolean preHandle(HttpServletRequest request,HttpServletResponse response,Object handler) throws  Exception{
		HttpSession session=request.getSession();

		if(session.getAttribute(LOGIN)!=null) {
			logger.info("clear login data before");
			session.removeAttribute(LOGIN);
		}
		return true;
	}
~~~
권한이 필요한 페이지에 접근시 로그인 페이지를 자동으로 띄우고 로그인성공시 쿠키에 정보를 저장하며 아래의 코드를 이용해 요청받은 URI를 저장해 이동시킨다.

```java
public boolean preHandle(HttpServletRequest request,HttpServletResponse response,Object handler) throws Exception{
		HttpSession session=request.getSession();

		if(session.getAttribute("login")==null) {
			logger.info("current user is not logined");
			saveDest(request);
			Cookie loginCookie=WebUtils.getCookie(request,"loginCookie");
			if(loginCookie!=null) {
				UserVO userVO=service.checkLoginBefore(loginCookie.getValue());
				logger.info("USERVO: "+userVO);
				if(userVO!=null) {
					session.setAttribute("login", userVO);
					return true;
				}
			}
			response.sendRedirect("/user/login");
			return false;
		}
		return true;
	}

	private void saveDest(HttpServletRequest req) {

		String uri=req.getRequestURI();

		String query=req.getQueryString();

		if(query==null||query.equals("null")) {
			query="";
		}else {
			query="?"+query;
		}

		if(req.getMethod().equals("GET")) {
			logger.info("dest: "+(uri+query));
			req.getSession().setAttribute("dest", uri+query);
		}
	}
```
권한이 필요한 페이지에 접속시 요청URI 를 저장하고 로그인처리시 해당 타겟으로 `sendRedirect()`시킨다.
