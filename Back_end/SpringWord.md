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

