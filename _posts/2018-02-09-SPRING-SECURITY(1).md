---
title: "[Spring] 스프링 시큐리티(1)"
date: 2018-02-09 11:30:00
categories:
- Spring
tags:
- Spring
- Spring-Security
---
**스프링 시큐리티를 이해하기 위해서 스프링 시큐리티가 무엇인이 알아야한다.**

# 스프링 시큐리티란 무엇인가?
스프링 시큐리티 레퍼런스에선 자바 EE기반의 엔터프라이즈 소프트웨어 어플리케이션을 위한 포괄적인 보안 서비스들을 제공하고 오픈 플랫폼이면서 자신만의 인증 매커니즘을 간단하게 만들 수 있다고 자랑한다.

>하지만 SBang프로젝트에서 이 플랫폼에 대해 뒤늦게 알고 적용해보려 했지만 기존의 코드의 전반적인 수정이 요했기 때문에 감히 건들지 못하고 포기한바 있다.

하지만 이번 TheMovie 에서는 기필코 적용해보고자 한다. 기본적으로 스프링 시큐리티는 인증(Authentication)과 권한(Authorization) 이 두가지이다. 쉽게말해 인증은 값을 입력해 권한은 얻는것(HTTP 기본인증이 그 예이다), 권한은 이 인증과정이 진행되었는지를 확인하는 부분이다.<br/>

또한 권한부여에도 2가지 영역: 웹 요청권한, 메소드 호출 및 도메인 인스턴스에 대한 접근 권한부여가 있다. 스프링 시큐리티는 이렇게 모든 중요한 영역에 필요한 기능을 제공한다.

# 스프링 시큐리티 시작
다음과 같이 메이픈 디펜던시를 추가한다.

![](/images/2018-02-09/spring_sec_pom.png)

## 자바 설정
스프링 시큐리티 레퍼런스에서는 자바기반의 설정으로 설명하고있다. 스프링 프레임워크 3.1에서부터 어노테이션을 통해 자바설정을 지원하기 때문에 3.2부터는 xml로 설정하지 않아도 된다. 원래 XML 기반의 설정에서는 web.xml에 org.springframework.web.filter.DelegatingFilterProxy라는 springSecurityFilterChain을 등록하는 것으로 시작 자바 기반의 설정에선 `WebSecurityConfigurerAdapter`를 상속받은 클래스에 `@EnableWebSecurity`어노테이션을 명시하는 것으로 springSecurityFilterChain가 자동으로 포함된다.<br/>

그 다음엔 `ApplicationIniticalizer`에 `WebSecurityConfigurerAdapter`를 상속받은 클래스를 `getRootConfigClasses()`메소드를 상속받는 것으로 기본설정이 끝난다.

![init](/images/2018-02-09/sec_init.png)

## HttpSecurity
그리고 configure메소드를 통해서 우리만의 인증 매커니즘을 구성해야한다.
**추가작업 요함**
