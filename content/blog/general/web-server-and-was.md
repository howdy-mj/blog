---
title: 'Web Sesrver & WAS'
date: 2020-8-13 23:10:00
category: 'general'
draft: true
---

## Web Server & WAS

최초의 인터넷은 단순한 정보 전달만 가능했지만 점차 서로 정보를 교환할 수 있게 되었고, 최근은 개인 맞춤형 콘텐츠 및 서비스 제공 시대까지 이르렀다. 이러한 인터넷의 발전에 따라 웹의 동작 원리도 바뀌었다.

---


### Web이란?

**웹**([World Wide Web, WWW](https://en.wikipedia.org/wiki/World_Wide_Web))은 인터넷에 연결된 사용자들이 정보를 공개 및 공유할 수 있는 공간이다. 웹은 크게 세 가지 기능으로 나뉜다.

1. 해당 정보를 웹에 출력 해주는 하이퍼텍스트 ex. HTML
2. 해당 공간의 위치(주소) ex. URL
3. 해당 문서나 자원에 접근하는 프로토콜 ex. HTTP
- 🔍용어: 하이퍼텍스트, HTML, URL, HTTP
    - 하이퍼텍스트(HyperText): 한 문서에서 다른 문서의 위치 정보를 심어서 양쪽을 연결해주는 텍스트
    - HTML(HyperText Language): 웹을 위한 마크업*(태그 등을 이용해 문서나 데이터 구조를 명기하는 언어 중 하나)* 언어
    - URL(Uniform Resource Locator): *https://www.rootenergy.co.kr/home* 와 같은 웹의 주소
    - HTTP(HyperText Transfer Protocol): 웹 상에서 정보(하이퍼텍스트 문서)를 주고 받을 때 사용 되는 통신 규약

### 웹의 역사와 발전

**WEB 1.0**

- 1989년 팀 버너스리가 연구원 간에 아이디어를 주고받을 때 항상 전자 우편이나 파일을 통해 주고 받는 것이 비효율적이라 생각하여 제안한 것으로, 공통된 공간에 각자의 정보를 올리고 관리할 수 있는 일종의 정보 관리 시스템에서 시작되었다.
- 정리된 자료를 전달받는 형태로, 주로 텍스트와 링크로 이루어졌다. (정적인 형태)

**WEB 2.0**

- 2000년대 초, 사용자들이 웹에 글을 쓰거나 동영상을 올리는 등 직접 생산하고 참여하는 공간으로 바뀌었다.
- 회원등록 및 조회, 메일링, 게시판, 방명록 등의 CGI 프로그램의 등장이 컸다고 본다.
- 🔍용어: CGI(Common Gateway Interface)
    - 웹 서버와 클라이언트 간의 정보 교환을 가능하게 해주는 프로그램으로, 양방향 정보교환을 가능하게 함
    - Perl, Java, PHP 등 언어로 제작 가능

**WEB 3.0**

- 2010년 이후, PC 뿐만이 아니라, 모바일, 태블릿 등 1인 1단말기가 보편화 되면서 개인화, 맞춤화 등의 인식이 생겼다. 컴퓨터 역시 시맨틱 웹(Semantic Web) 기술을 이용해 웹 페이지에 담긴 내용을 이해하고 사용자에게 맞춤형 콘텐츠 및 서비스를 제공할 수 있게 되었다.
- 🔍용어: 시맨틱 웹
    - 시맨틱: 페이지의 태그를 통해 의미 부여를 할 수 있는 기능

    시맨틱 웹은 '의미론적인 웹'이라는 뜻으로, 인터넷 상의 리소스(웹 문서, 파일, 서비스 등)에 대한 정보와 자원 사이의 정보를 기계가 처리할 수 있는 온톨로지 형태로 표현하고, 이를 자동화된 기계(컴퓨터)가 처리하도록 하는 프레임워크이자 기술이다. 

    HTML5에서 시맨틱 웹을 쉽게 구성할 수 있도록 시맨틱 태그 요소들이 추가되었다. 

## Web Server

**웹 서버**는 사용자들이 요청하는 정적인 페이지를 보여줄 때 사용된다. 정적인 페이지는 WEB 2.0까지 주로 다뤄졌던 HTML, 파일, 이미지, 비디오 등의 컨텐츠로 이루어진 페이지이다. 

- 🔍용어: 서버
    - 서버: 클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 프로그램 또는 장치

대표적인  Web Server에는 Nginx(ex. Dropbox, Netflix, Wordpress.com 등), Apache HTTP Server가 있다. 

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48d1c58b-3bdc-448d-9bc4-43b8a398dd36/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48d1c58b-3bdc-448d-9bc4-43b8a398dd36/Untitled.png)

## WAS(Web Application Server)

Web Server와 WAS의 가장 큰 차이는 비즈니스 로직(동적인 데이터 생성 및 작업) 처리 가능 여부이다. 그러기 위해서 웹 애플리케이션이 무엇인지 먼저 알아보자. 

- 🔍용어: 비즈니스 로직(Business Logic)
    - 동적 데이터 생성 및 작업
    - 클라이언트가 원하는 데이터를 보여주기 위해, 데이터베이스에 연결, 생성, 변경, 저장하는 작업
    - ex. 회원 가입을 위해 아이디 작성 후 제출하면, 데이터베이스에 중복된 아이디가 있는지 연결하여 확인 및 결과를 클라이언트에 전달

**Web Application**

**웹 애플리케이션**은 웹상에서 단순히 동적으로 페이지를 출력하는 것 보다, 웹에서 특정 업무를 처리하는 것을 뜻한다. 예를 들면, 어떠한 소스 파일을 특정 파일로 변환, 회사 실무 관리나 DB 관리, 혹은 HTML5 Canvas를 이용한 게임 등이 있다. 

WAS는 Web Server + Web Container를 말한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5151346-987c-4cf3-a6f4-e91629759921/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5151346-987c-4cf3-a6f4-e91629759921/Untitled.png)

웹 서버는 정적인 페이지를 보여줄 때 사용 되는 것이니, 웹 컨테이너가 동적인 것을 처리하는 것이라 유추할 수 있다. **웹 컨테이너**(혹은 서블릿 컨테이너)는 Servlet, JSP를 실행시킬 수 있는 소프트웨어로 서버 환경을 제공한다. 인터넷에서 HTTP를 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행 해주는 미들웨어이다. 

HTTP 요청을 받아서 Servlet, JSP 등을 실행시켜 클라이언트와 데이터베이스가 통신할 수 있도록 처리해주는 미들웨어.

- 🔍용어: Servlet, JSP, 미들웨어
    - Servlet: 자바를 사용하여 웹을 동적으로 생성하는 서버 측 프로그램 혹은 그 사양 (Java로 작성)
    - JSP: HTML 내에 자바 코드를 삽입하여 웹 서버에서 동적으로 웹 페이지를 생성하여 웹 브라우저에 돌려주는 언어. 실행 시 자바 서블릿으로 변환 후 실행되어 서블릿과 유사하지만, 서블릿과 달리 HTML 표준에 따라 작성되어 웹 디자인에 용이
    - 미들웨어: 사용자와 데이터베이스 사이에서 데이터를 주고 받을 수 있게 해주는 매개 역할로, 대부분의 로직을 처리 함

### WAS 동작 원리

1. 웹 서버로부터 요청(동적)이 오면 웹 컨테이너가 이를 받아 맞는 Servlet을 메모리에 올린다.
2. 웹 컨테이너는 web.xml을 참조해 해당 Servlet에 대한 Thread를 생성하고, HttpServletRequest와 HttpServletResponse 객체를 생성하여 전달한다. 
3. 웹 컨테이너는 서블릿을 호출한다. 
4. 호출된 서블릿의 작업을 담당하게 된 쓰레드는 그에 맞는 doGet() 또는 doPost()를 호출해 생성된 동적 페이지를 Response 객체에 담아 WAS에 전달한다. 
5. 웹 컨테이너는 전달받은 Response 객체를 HttpResponse 형태로 바꿔 웹 서버에 전달하고, 생성되었던 쓰레드를 종료하고 HttpServletRequest, HttpServletResponse 객체를 제거한다. 
6. 웹 컨테이너에서 처리된 결과는 웹 서버로 전달되고 웹 서버는 결과 값을 받아 정적 데이터 처리를 한다. 

### WAS의 주요 기능

1. 프로그램 실행 환경과 데이터베이스 접속 기능을 제공
2. 여러 개의 트랜잭션(논리적인 작업 단위) 관리
3. 업무를 처리하는 비즈니스 로직 수행
4. 웹 서버의 기능도 기본적으로 포함
5. 프로그램의 동적 결과를 웹 브라우저에게 전달

WAS는 대부분 Java 기반으로 주로 Java EE 표준을 수용하고 있으나, Java EE 표준을 따르지 않는 제품과 .NET이나 Citrix 기반인 비 자바 계열도 존재한다. 

대표적인 WAS에는 Apache Tomcat, Glassfish 등이 있다. 

## Web Server와 WAS의 차이점

Web Server

- 정적 콘텐츠(HTML, CSS, 이미지, 비디오 등)
- HTTP 프로토콜만 사용
- 웹 기반 애플리케이션
- 멀티쓰레드 미지원

WAS

- 동적
- 비즈니스 로직을 여러 프로토콜(HTTP 포함)을 통해 제공
- 웹과 엔터프라이즈 기반 애플리케이션
- 멀티쓰레드를 통해 병렬처리 지원

### 와이어샤크를 통한 분석

### Node + NginX

docker로 진행

[https://docs.docker.com/docker-for-mac/](https://docs.docker.com/docker-for-mac/)

### Tomcat

[https://whitepaek.tistory.com/12](https://whitepaek.tistory.com/12)

[https://hub.docker.com/_/tomcat](https://hub.docker.com/_/tomcat)

# 왜 Web Server와 WAS를 같이 사용하는가?

WAS는 Web Server의 역할을 포함하고 있기 때문에 WAS만 사용해도 문제는 되지 않는다. 심지어 Web Server 없이 WAS로만 정적 콘텐츠를 처리하는 것도 고작 10% 정도 느릴 뿐 문제가 엄청 크지 않다.  하지만 **확장성**을 위해서 WAS 앞 단에 Web server를 둔다. 

하지만 만약 많은 사람들이 동시에 사용하면서  WAS를 재시작해야 할 때, 사용자들이 웹 서버에서 먼저 해당 WAS를 이용하지 못하도록 하고, WAS를 재시작한다면 해당 웹 애플리케이션을 사용하는 사람은 WAS에 문제가 발생했는지 모르고 이용가능하다. 이러한 처리를 장애 극복 기능이라 한다. 대용량 웹 애플리케이션에는 무중단으로 운영하기 위해 상당히 중요한 기능이다. 이러한 기능 때문에 보통 웹 서버가 WAS 앞단에서 동작하도록 하는 경우가 많다. 

1. 로드밸런싱(Load Balancing)을 위해 사용
    - 로드밸런싱은 한 대의 서버에 부하가 집중되지 않도록 트래픽을 분산하는 장치 또는 기술이다.

fail over(장애 극복), fail back 처리에 유리

대용량 웹 애플리케이션의 경우(여러 개의 서버 사용) 웹 서버와 와스를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응(예, 앞단의 웹 서버에 오류가 발생한 와스를 이용하지 못하도록 한 후, 와스를 재시작하여 사용자는 오류를 느끼지 못하고 이용할 수 있다.)

여러 웹 애플리케이션 서비스 가능(예, 하나의 서버에 php와 java를 함께 사용)

즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 분리한다. 

[http://www.narendranaidu.com/2006/02/why-put-webserver-in-front-of.html](http://www.narendranaidu.com/2006/02/why-put-webserver-in-front-of.html)

- Webservers serve static content faster than application servers. Hence for performace reasons, it makes sense to shift all the static contect to the webserver. (J2EE developers hate this, as they now have to split the *.war files)
- The webserver can be put in a DMZ to enable enhanced security.
- The webserver can be used as a load balancer over multiple application server instances.
- The webserver can have agents/plug-in's to security servers such as Netegrity. Hence security is taken care by the webserver, putting less load on the application server.

- HTTPS endpoint - nginx and Apache httpd can handle HTTPs better than your software can, and you are using HTTPs, aren't you
- Security - httpd and nginx are mature and well supported. Putting httpd or nginx in front of your software protects it from probe attacks, hides its identity and can manage access to it.

[https://people.apache.org/~mturk/docs/article/ftwai.html](https://people.apache.org/~mturk/docs/article/ftwai.html)

[https://sethuramanmurali.wordpress.com/2013/06/19/why-we-need-web-server-in-front-of-application-server/#:~:text=Because while Tomcat may be,the functioning of your application](https://sethuramanmurali.wordpress.com/2013/06/19/why-we-need-web-server-in-front-of-application-server/#:~:text=Because%20while%20Tomcat%20may%20be,the%20functioning%20of%20your%20application).

참조

- [https://hack-cracker.tistory.com/146](https://hack-cracker.tistory.com/146)
- [https://www.ibm.com/cloud/learn/web-server-vs-application-server](https://www.ibm.com/cloud/learn/web-server-vs-application-server)
- [https://www.educative.io/edpresso/web-server-vs-application-server](https://www.educative.io/edpresso/web-server-vs-application-server)
- [https://www.edwith.org/boostcourse-web/lecture/16666/](https://www.edwith.org/boostcourse-web/lecture/16666/)
- [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
- [https://passionha.tistory.com/341](https://passionha.tistory.com/341)
- [https://jeong-pro.tistory.com/84](https://jeong-pro.tistory.com/84)