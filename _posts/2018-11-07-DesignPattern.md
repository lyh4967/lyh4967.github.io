---
title: "[DesignPattern] 디자인 패턴"
date: 2018-11-07 11:30:00
categories:
- DesignPattern
tags:
- DesignPattern
---
**여러 디자인 패턴에 대해 알아봅시다**

# Iterator

# Adapter
기존의 클래스를 전혀 수정하지 않고 목적한 인터페이스에 맞추려는 것이다.

## 상속과 인스터스

# Factory Method
**인스턴스를 생성하는 공장이다.**<br/>
하나의 framework를 만들어 놓고 그 틀을 상속받아 구체적인 속성을 가진 클래스를 구현한다.

# Singleton
**인스턴스를 1개만 생성하고 싶다. ex)System, 시스템 설정**<br/>
singleton은 static필드로서 클래스의 인스턴스에서 초기화된다.
* 클래스가 초기화 될때 1번만 인스턴스가 생성된다.
* 생성자는 private로 선언

```java
public class Singleton {
	private static Singleton singleton=new Singleton();
	private Singleton() {
		System.out.println("인스턴스를 생성했다.");
	}
	public static Singleton getInstance() {
		return singleton;
	}
}
```

# Prototype
**클래스로부터 인스턴스를 생성하는 것이 아닌, 원래있는 인스ㅓㄴ스를 복사해서 새로운 인스턴스를 생성한다.**<br/>

## 필요성
1. 취급하는 오브젝트의 종류가 너무 많아 각각을 별도의 클래스로 만들어 다수의 소스로 작성해야할때.

2. 인스턴스를 생성하는 작업자체가 굉장히 복잡할때

3. framework를 특정 클래스에 의존하지 않도록 만들고 싶을때, '모형'의 인스턴스를 등록하고 그 인스턴스를 복사해서 사용한다.

```Java
public Product createClone() {
	Product p=null;
	try {
		p=(Product)clone();
	}catch(CloneNotSupportedException e) {
		e.printStackTrace();
	}
	return p;
}
```
clone()라는 메서드는 Object클래스에 속해있는 메서드로 Clonable 인터페이스를 구현한 클래스만 사용이 가능하다.

# Builder
Builder이라는 가장 큰 틀을 정해놓고 그 틀에 따라 만들어낼 구체적이 클래스를 작성한다. ex)TextBuilder, HTMLBuilder<br/>
그리고 입력으로 프로그램의 타이틀과 내용을 받고 특정 Builder을 정해서 Builder을 넣고 프로그램을 구동한다.
**String와 StringBuffer, StringBuilder의 차이**</br>
String 는 한번 메모리 공간이 할당되면 변하지 않는다. 따라서 문자열을 수정할때 new로 만들어서 새로운 공간을 생성한다.따라서 기존의 메모리공간은 가비지컬렉터에의해 제거되어져야한다.</br>
또한 StringBuffer과 StringBuilder은 synchronized 지원여부이다. StringBuffer는 동기화가 지원되기 때문에 멀티쓰레드 환경에서 적합하다. 하지만 StringBuilder은 그렇지 않다.

# Abstract Factory
## 관련부품 조합해서 제품만들기
기존과는 달리 구체적인 구현이 아닌 인터페이스에 주목한다. 또한 인터페이스만을 사용해서 부품을 조립하고 제품으로 완성한다.
**안했음**

# Bridge
**'기능의 클래스 계층'과 '구현의 클래스 계층'사이에 다리를 놓는다.**<br/>

기능을 추가하려는것인가? 구현을 수행하려는것인가? 이것은 계층을 복잡하게해 예측을 어렵게할 우려가 있다. 따라서 클래스 계층을 분리하고 이를 연결시킨다.이렇게 계층을 분리해 놓으면 클래스 계층을 독립적으로 확장할 수 있다.

# Strategy
각 문제에 대해 알고리즘을 교체해서 사용하기위한 패턴이다.<br/>
```Java
public class Main{

	public static void main(String[] args)  {
		Player player1=new Player("두리",new WinningStrategy(seed1));//무작정 전략
		Player player2=new Player("하나",new ProbStrategy(seed2));//확률기반 전략

	}
}
```
각 플레이어의 전략은 Strategy 인터페이스를 구현한 구체적 전략(알고리즘)들을 활용해 프로그램을 실행한다.
**알고리즘과 다른 부분을 의식적으로 분리해 알고리즘의 인터페이스 부분만을 규정한다**<br/>
Player클래스에 Strategy인터페이스라는 위임으로 느슨한 연결을 사용해서 용이하게 알고리즘을 교체할 수 있다.

# Composite
**상자안의 상자구조**<br/>
예) 폴더안에는 폴더도 들어갈 수 있고 파일도 들어갈 수 있다. 따라서 Entry라는 추상클래스를 상속받은 Directory와 File을 구현하고 Directory에 Entry가 들어갈 수 있는 형태의 패턴으로 재귀적 표현

# Deccorator
중심이 되는 오브젝트에 장식이 되는 기능을 하나씩 추가하면서 좀더 목적에 맞는 오브젝트를 생성한다.<br/>
장식이 되는 클래스 안에 내용물 클래스를 위임하고 계속해서 내용물을 감싼다
```Java
BufferedReader br=null;
try{
	br=new BufferedReader(new FileReader(new File("textFile.txt")));
}catch (FileNotFoundException e){
	e.printStackTrace();
}
```
