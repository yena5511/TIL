## Spring

#### 1.1 스프링의 개념
스프링은 자바 기반의 웹 어플리케이션을 만들 수 있는 프레임워크이다.
스프링의 구조는 아래와 같은 구조로 이루어져있다.
![Alt text](https://melonicedlatte.com/assets/images/2021_3Q/spring_architect.png)

#### 1.2 스프링의 특징
Spring은 자바 객체와 라이브러리들을 관리해주며, 톰캣과 같은 WAS 가 내장되어 있어 자바 웹 어플리케이션을 구동할 수 있다.<br>
spring은 경량 컨테이너로 자바 객체를 직접 spring 안에서 관리한다. 객체의 생성 및 소멸과 같은 생명 주기를 관리하며, spring 컨테이너에서 필요한 객체를 가져와 사용한다.<br>
● Spring의 가장 큰 특징으로 IOC와 DI가 많이 언급됩니다. IOC와 DI의 간단한 개념은 아래와 같습니다.<br>
    ● 제어의 역전 (IOC, Inversion Of Control)<br>
         - 일반적으로 처음에 배우는 자바 프로그램에서는  모든 작업을 사용자가 제어하는 구조였다. 예를 들어 A 객체에서 B 객체에 있는 메소드를 사용하고 싶으면, B 객체를 직접 A 객체 내에서 생성하고 메소드를 호출합니다.<br>
         - 하지만 IOC가 적용된 경우, 객체의 생성을 특별한 관리 위임 주체에게 맡깁니다. 이 경우 사용자는 객체를 직접 생성하지 않고, 객체의 생명주기를 컨트롤하는 주체는 다른 주체가 됩다. 즉, 사용자의 제어권을 다른 주체에게 넘기는 것을 IOC(제어의 역전) 라고 합니다.<br>
         - 요약하면 Spring의 Ioc란 클래스 내부의 객체 생성 -> 의존성 객체의 메소드 호출이 아닌, 스프링에게 제어를 위임하여 스프링이 만든 객체를 주입 -> 의존성 객체의 메소드 호출 구조입니다. 스프링에서는 모든 의존성 객체를 스프링이 실행될때 만들어주고 필요한 곳에 주입해줍니다.<br>
    ● 의존성 주입 (DI, Dependency Injection)<br>
         - 어떤 객체(B)를 사용하는 주체(A)가 객체(B)를 직접 생성하는게 아니라 객체를 외부(Spring)에서 생성해서 사용하려는 주체 객체(A)에 주입시켜주는 방식이다. 사용하는 주체(A)가 사용하려는 객체(B)를 직접 생성하는 경우 의존성(변경사항이 있는 경우 서로에게 영향을 많이 준다)이 높아진다. 하지만, 외부(Spring)에서 직접 생성하여 관리하는 경우에는 A와 B의 의존성이 줄어든다.<br>

## Spring Boot

#### 스프링 부트의 개념

스프링 부트(Spring Boot)는 스프링(Spring)을 더 쉽게 이용하기 위한 도구라고 볼 수 있다. 스프링 이용하여 개발을 할 때, 이것저것 세팅을 해야 될 요소들이 많다. 여러가지를 세팅해야되는 진입 장벽이 존재하여 Spring 을 처음 배우려는 사람들은 중도에 그만두는 경우가 많다고 한다. Spring Boot는 매우 간단하게 프로젝트를 설정할 수 있게 하여, Spring 개발을 조금 더 쉽게 만들어주는 역할을 하고 있다.

## IoC(Inversion of Control)
: 프레임워크 기반의 개발에서는 프레임워크가 필요할 때마다 애플리케이션 코드를 호출하여 사용하는데
이 때 제어권을 가지는 것이 컨테이너(Container)이다.
객체에 대한 제어권을 컨테이너가 가지고 객체의 생성부터 생명주기 관리 등을 컨테이너가 맡아서 하면서
제어권의 흐름이 개발자에서 컨테이너로 바뀌었다고 해서 IoC(Inversion of Control)라고 부름
#### IOC 용어
- bean : 스프링에서 제어권을 가지고 직접 만들어 관계를 부여하는 오브젝트(스트링 컨테이너가 생성하고 관계설정, 사용을 제어해주는 오브젝트)
- bean factory : 스프링의 IoC를 담당하는 핵심 컨테이너
Bean을 등록/생성/조회/반환/관리 (보통 bean factory를 바로 사용하지 않고 이를 확장한 application context를 이용)
BeanFactory는 bean factory가 구현하는 interface이다. (getBean()등의 메서드가 정의되어 있다.)
- application context : bean factory를 확장한 IoC 컨테이너
bean factory에 추가로 spring의 각종 부가 서비스를 제공ApplicationContext는 application context가 구현해야 하는 interface, BeanFactory를 상속
- container (ioC container) : IoC 방식으로 bean을 관리한다는 의미(bean factory나 application context를 가리킴)
하나의 애플리케이션에 보통 여러개의 ApplicationContext 객체가 만들어짐-> 모두 합해서 spring container라고 부를 수 있다.

#### 의존 주입 DI(Dependency Injection)
: 어떠한 객체가 다른 객체를 필요로 함에 따라 setter나 생성자를 사용하여 필요로 하는 객체를 주입시켜 기능을 사용할 수 있게 하는 것 (또는 직접 주입 가능)

#### WAS (Web Application Server)
: WAS는 일종의 미들웨어로 웹 클라이언트(보통의 웹 브라우저)의 요청 중 웹 애플리케이션이 동작하도록 지원하는 목적을 가진다.(Tomcat)

## Domain Driven Design(DDD)

- Domain: 온라인서점 사이트에서 책을 조회하고 구매한다고 가정하자. 개발자 입장에서 온라인서점은 구현해야할 소프트웨어의 대상이 된다. 온라인서점 소프트웨어는 상품의 조회, 구매, 결제등의 기능을 제공해야한다. 이때 온라인 서점은 소프트웨어로 해결하고자하는 문제 영역, 즉 도메인(domain)에 해당된다.

- Entity: Entity모델에서의 객체는 `Entity`이고
`Entity`는 다른 Entity와 구별할 수 있는 `식별자`를 가진다.
Entity는 객체의 상태가 계속 변한다.
Entity의 `식별자`는 고유하기 때문에 `도메인`에서 개별성이 있는 개념이라고 생각하면 된다.


- Value Object: Value Object는 값 객체라고 불린다.
Value Objec는 값 속성이 개별적으로 변하지 않고 값 자체로 고유한 객체를 뜻한다.
엔티티가 `id`로 고유성을 가진다면 Value는 `값의 조합`으로 고유성을 가진다.
그리고 한 번 만들어지면 내용이 변하지 않는다.

#### API
:Application Programming Interface의 줄임말로 응용 프로그램에서 사용할 수 있도록, 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻합니다.

#### Interface
:클라이언트 프로그램에서 구현을 보지 않고도 객체를 사용할 수 있는 매개체 역할을 하고, 공통적인 목적을 갖고있는 클래스들의 틀을 제공하는 역할을 한다. 
 
 #### Controller
 :Controller는 창구 역할을 하는 API들을 모아놓은 클래스이다.
당연히 정보를 프론트 쪽으로 내려줄 때도 이 컨트롤러의 API를 통해서 보내준다.


#### Repository
: Repository는 직역해도 '저장소'로 데이터베이스와 깊은 연관이 있음을 알 수 있다. 데이터단에 직접 매칭되는 Entity라는 것이 있는데, 이 Entity를 통해 데이터 테이블이 생성이 되면, 받아온 정보를 데이터베이스(ex. MySQL, mariaDB)에 저장하고 조회하는 기능을 수행한다.

#### Service
: Service는 위에서 언급했듯이 Repository에서 얻어온 정보를 바탕으로 자바 문법을 이용하여 가공 후 다시 Controller에게 정보를 보내는 곳이다.
어떻게 보면 Controller는 클라이언트에, Repository는 데이터에 맞닿아서 정보를 주고받는 부분으로 여길 수 있으나 실질적으로 중요한 작동이 많이 일어나는 부분이라고 할 수 있다.


컨트롤러 : @Controller (프레젠테이션 레이어, 웹 요청과 응답을 처리함) <br>
로직 처리 : @Service (서비스 레이어, 내부에서 자바 로직을 처리함) <br>
외부I/O 처리 : @Repository (퍼시스턴스 레이어, DB나 파일같은 외부 I/O 작업을 처리함)


#### JDBC
: Java에서 데이터베이스에 접근할 수 있는 Java 인터페이스를 의미한다.


#### @GetMapping
- @RequestMapping(method = RequestMehtod.GET)의 축약형으로써, 어노테이션만 보고 무슨 메소드 요청인지 바로 알아볼 수 있다.
- @GetMapping은 요청 URL을 어떠한 메서드가 처리할 지 매핑한다.
- Controller 내부에서 URI 경로를 지하정는 역할도 한다.



#### Factory
:  특정 오브젝트를 요구하면서 오브젝트를 생성하거나 가져오는 방식을 신경쓰지 않도록 중간에서 역할하는 오브젝트


#### AOP
핵심 로직과 부가 기능을 분리하여 애플리케이션 전체에 걸쳐 사용되는 부가 기능을 모듈화하여 재사용할 수 있도록 지원하는 것

#### JPA
: JPA란 Java Persistence API의 약자이며 자바의 ORM을 위한 표준 기술로 Hibernate, Spring JPA, EcliplseLink 등 과 같은 구현체가 있고 이것의 표준 인터페이스가 JPA 이다.

- ORM(Object-Relational Mapping)이란 자바의 객체와 관계형 DB를 맵핑하는 것으로 DB의 특정 테이블이 자바의 객체로 맵핑되어 SQL문을 일일이 작성하지 않고 객체로 구현할 수 있도록 하는 프레임워크입니다.

- 장점
     - SQL 위주의 Mybatis 프로젝트와 비교하여 쿼리를 하나하나 작성할 필요도 없어 코드량이 엄청나게 줄어든다. 
     - 객체 위주로 코드가 작성되다 보니 가독성도 좋고, 여러 가지 요구사항으로 기능 수정이 발생해도 DB부터 더 간편하게 수정이 가능하다.
     - Oracle, MySQL 등 DB 벤더에 따라 조금씩 다른 SQL 문법 때문에 애플리케이션이 DB에 종속될 수밖에 없었는데, JPA는 직접 쿼리를 작성하는 것이 아니라서 DB 벤더에 독립적으로 개발이 가능하다.