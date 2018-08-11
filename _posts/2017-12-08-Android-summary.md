---
title: "[Android] 안드로이드 요약"
date: 2017-12-08 10:10:00
categories:
- Android
tags:
- ANDROID
- SUMMARY
---
**이번 과정동안에 안드로이드를 적용할 계획은 없지만 후에 사용될 경우를 위해 맛보기로 해본다.**

#안드로이드 스튜디오의 설치 -  [안드로이드 설치](developer.android.com)
-다운로드후, 설치시 C:\Android\Android Studio로 설치경로를 변경.
-SDK(개발도구)의 설치 경로 역시 c:\android\sdk로 변경 SDK(개발도구)는 최신이 설치됨. 나중에 필요한 것만 설치가능. AVD를 기본으로 생성해줌. 추가가능.
>AVD(Android Virtual Device) : 소프트웨어로 제작된 안드로이드폰 에뮬레이터

#안드로이드의 역사
developer.android.com
-잘 발전하다가 3.0 허니콤에서 성급하게 태블릿 전용 SDK를 발표한다. 프레그먼트, 액션바 등의 새로운 기술이 추가되었지만, SDK가 폰과 태블릿 용으로 갈라져 일관성이 훼손되었다.3.x SDK는 이제 완전히 폐기 되었으며, 4.0부터 하나의 SDK로 통합되었다.개발도구를 이클립스에서 IntelliJ기반으로의 변경. 4.x인 Jelly Bean부터 테스트해오다가.2014.10에 발표된 5.0 롤리팝'부터 IntelliJ기반의 Andrioid Studio(Gradle기반)을 사용. 안드로이드 전용이라 통합성이 뛰어나다.그리고 런타입이 달빅(Dalvik)에서 ART(Android RunTime)로 바뀌었다.

예전에는 DEX라는 바이트코드로 컴파일하고 실행할 때 JIT로 번역하여 실행.ART는 앱이 설치될 때 AOT(Ahead Of Time)컴파일러가 DEX파일을 기계어로 일괄번역하여 타깃 장비에서 바로 실행할 수 있는 실행파일로 변환해 둔다. 실행할 때 번역 과정을 거치지 않고 네이티브 리눅스 프로세스로 실행되므로 훨씬 속도가 빠르다.그리고 ART는 GC기능을 개선. 프로파일러와 디버거 성능도 크게 보강. 기존 달빅에서 돌아가던 앱도 ART에서 잘실행되지만 JNI나 3rd 파티기능에는 문제가 있을 수 있다.

#### 3. 첫번째 안드로이드 앱-File > New > New Project...  Empty Activity선택
**app > java > com.example.myfirstapp > MainActivity.java**
기본 activity입니다(앱의 진입점). 앱을 빌드하고 실행하면, 이 Activity의 인스턴스가 시작되고 해당 레이아웃을 로드합니다.
<br/>

**app > res > layout > activity_main.xml**
XML 파일은 activity UI의 레이아웃을 정의하며, "Hello world!" 텍스트가 있는 TextView 요소를 포함합니다.
<br/>

**app > manifests > AndroidManifest.xml**
매니페스트 파일은 앱의 기본 특징을 설명하고 앱의 각 구성요소를 정의합니다.Gradle Scripts > build.gradle이 이름을 가진 파일이 2개 있음. 하나는 프로젝트용. 다른 하나는 "앱" 모듈용각 모듈에는 자체 build.gradle 파일이 있지만, 현재 이 프로젝트에는 하나의 모듈만 있음.Gradle 도구에서 앱을 컴파일하고 빌드하는 방법을 구성하기 위해 대부분 모듈의 build.gradle 파일을 사용

[Tip] 안드로이스 스튜디오(이하 안스)에서 프로젝트를 지우려면, 프로젝트를 닫고 탐색기에서 해당 폴더를 직접 삭제해야함.

# 안드로이드의 4가지 구성요소
안드로이드의 실행파일은 같은 패키지에 속한 자바 클래스와 리소스의 집합일 뿐 프로세스와 반드시 대응되지 않는다.

대부분 실행파일이 곧 프로세스이지만 안드로이드에서는응용프로그램끼리 서로의 기능을 공유할 수 있고 다른 프로그램의 구성요소를 불러와 같은 주소 공간에서 실행되기도 한다. 윈도우즈의 COM이나 OMG의 CORBA와 개념적으로 유사하되 다만 로컬 내부에서만 기능을 공유한다는 점이 다르다.


안드로이드 응용프로그램은 적절한 권한만 있으면 언제든지 인스턴스화할 수 있는 4개의 주요 컴포넌트로 구성된다.그래서 **main과 같은 유일한 진입점이 따로 없으며 처음 생성되는 인스턴스의 생성자가 실질적인 진입점이다.**

안드로이드 응용 프로그램을 구성하는 4개의 컴포넌트는 다음과 같다.


 * 액티비티 - 일반적인 화면(윈도우). View와 ViewGroup으로 구성
 * 서비스 - 백그라운드 작업
 * 방송 수신자(BR) - 방송 수신한 후, 의미를 해석해서 적절한 액티비티나 서비스를 띄움.
 * 컨텐츠 제공자(CP) - 프로그램간에 데이터(주소록)를 공유할 수 있는 유일한 장치

응용프로그램은 이들 컴포넌트 중 일부 또는 여러 개를 가진다.응용 프로그램의 컴포넌트 구성은 메니페스트라는 설정파일에 저장 및 관리되며 최초 실행시 어떤 액티비티를 띄울 것인가도 메니페스트에서 지정한다.

# 두번째 안드로이드 앱 - [연습링크](https://developer.android.com/training/basics/firstapp/creating-project.html)

새로운 Activity를 만들면 Android Studio는 다음 세 가지 작업을 자동으로 수행합니다.
 - DisplayMessageActivity.java 파일을 생성합니다.
 - 해당 activity_display_message.xml 레이아웃 파일을 생성합니다.
 - 필수 <activity> 요소를 AndroidManifest.xml에 추가합니다.

# 안스에서 자주쓰는 단축키

Alt+숫자 : 특정 창을 열기/접기(토글) - 창옆에 숫자가 적혀있음.(창간의 이동은 Alt+ 좌우화살표키)

Ctrl+Tab : 창간의 이동 및 선택(소스파일 포함)

Ctrl+F : 소스파일에서 찾기

Ctrl+Shift+F : 프로젝트 전체에서 찾기

Ctrl+N :클래스 찾기

Ctrl+Shift+N : 파일찾기(xml파일 찾을 때 유용)

Shift+F10 : 프로젝트 실행

F4 : 특정 대상의 소스(선언부)로 이동(소스파일에서 특정 클래스나 메서드의 이름에 커서를 놓고)

Ctrl+Shift+Backspace : 소스파일에서 마짐가 수정위치로 이동

Ctrl + G : 소스파일에서 해당 라인으로 이동

Ctrl + H : 선택된 클래스의 계층구조(Hierarchy)를 보여준다.

Ctrl+Shift+H : 선택된 메서드의 계층구조를 보여준다.

Ctrl+D : 현재 라인을 복사한다.

Ctrl+Space : 자동완성목록을 보여준다.

Ctrl+/ : 라인단위 주석처리/해제(토글)

Ctrl+Shift+/ : 블럭단위 주석처리/해제(토글)

Ctrl+Alt+o : 자동 import

Ctrl+Alt+I : 자동 들여쓰기

Ctrl+Alt+L : 자동 정렬하기

[에뮬레이터]

Ctrl+드래그 : Ctrl키를 누른 상태에서 마우스를 드래그하면 확대,축소가 가능하다.(pinch)

[참고] Android API문서 - https://developer.android.com/reference/packages.html

위의 글은 비트프로젝트과정 강사님께서 올려주신 글을 가져온 것입니다.
