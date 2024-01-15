## Bean

spring ioc 컨테이너가 관리하는 객체를 빈이라고 한다.

우리가 알던 기존의 Java에서는 Class를 생성하고 new를 입력하여 원하는 객체를 직접 생성한 후에 사용한다.
하지만 spring에서는 직접 new를 이용하여 생성한 객체가 아니라, spring에 의하여 관리간하는 자바 객체를 사용한다.
이렇게 spring에 의하여 생성되고 관리하는 자바 객체를 Bean이락 한다.
spring에서는 spring bean을 얻기 위해 ApplicationContext.getBean()와 같은 메서드를 사용하여 spring에서 직접 자바 객체를 얻어서 사용한다.


#### 자바 어노테이션을 사용하는 방법

java에서 Annotation은 자바 소스 코드를 추가하여 사용할 수 있는 메타데이터의 일종이다. 소스코드를 추가하면 단순 주석의 기능을 하는 것이 아니라 특별한 기능을 사용할 수 있다.

java에서 @Override, @Deprecated와 같은 Annotation을 제공한다. 아래의 상속 예제에서는 @Override를 이용하여 상속임을 명시한다.

```java
public class Parent { 
    public void doSomething() { 
        System.out.println("This is Parent"); 
    } 
} 

public class Son extends Parent{ 
    @Override 
    public void doSomething() { 
        System.out.println("This is Son"); 
    } 
}
```

spring에서는 여러 가지 Annotation을 사용하지만 bean을 등록하기 위헤서는 @Component Annotation을 사용한다.
@Component Annotation이 등록되어 있는 경우에는 Spring이 Annotation을 확인하고 자체적으로 Bean으로 등록한다.

실제로 사용되는 예시

실제 spring 프로젝트에서 Controller를 등록할 때에는 아래와 같은 Annotation을 사용한다.
아래의 예시에서 Controller임을 spring에게 알려주기위하여 @Controller Anntation을 사용했다.

```java
// HelloController.java
@Controller
public class HelloController {
    // Http Get method 의 /hello 경로로 요청이 들어올 때 처리할 Method를 아래와 같이 @GetMapping Annotation을 사용하여 Mapping을 사용할 수 있습니다.
    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "This is data!!");
        return "hello";
    }
}
```

@Controller Annotation을 intelliJ에서 Ctrl 을 눌러서 이동해보면 아래와 같은 소스를 확인할 수 있다.
 @Controller Annotation에는 @Component Annotation이 있는 것을 확인할 수 있다. 
 @Component Annotation 으로 인하여 Spring은 해당 Controller를 Bean 으로 등록한다.

#### Bean Configuration File에 직접 Bean 등록하는 방법

@Configuration과 @Bean Annotation 을 이용하여 Bean을 등록할 수 있습니다. 아래의 예제와 같이 @Configuration을 이용하면 Spring Project 에서의 Configuration 역할을 하는 Class를 지정할 수 있습니다. 
해당 File 하위에 Bean 으로 등록하고자 하는 Class에 @Bean Annotation을 사용해주면 간단하게 Bean을 등록할 수 있습니다.

```java
// Hello.java
@Configuration
public class HelloConfiguration {
    @Bean
    public HelloController sampleController() {
        return new SampleController;
    }

```

####  BeaDefinition

- 스프링 프레임워크에서 빈의 정의를 나태내는 구성 메타데이터이다.
이는 빈의 클래스, 범위, 라이프사이클 콜백, 의존성 및 기타 구성 세부 정보에 대한 정보를 제공한다.
- 스프링 컨테이너가 제공된 구성에 따라 빈 인스턴스를 생성하고 관리하는 데 사용된다.
이는 런타임에서 빈 객체를 생성하기 위한 역할을 한다.

#### BeanFactory

- 스프링의 핵심 인터페이스로, 빈의 생성, 관리, 검색 등을 담당한다.
- 빈을 등록하고 필요한 시점에 빈을 가져와서 사용할 수 있다.
빈의 지연 로딩을 지원하고, XML 또는 annotation기반으로 빈을 설정할 수 있다.

#### ApplicationContext

- BeanFactory의 하위 인터페이스로, 빈 컨테이너의 기능을 확장한 것이다.
- BeanFactory의 모든 기능을 포함하며, 추가적인 기능을 제공한다.
예를 들어, ApplicationContext는 메시지 리소스 번들, 이벤트 발생 및 처리, AOP등의 기능을 지원한다.
- 또한 ApplicationContext는 다양한 형식으로 빈을 설정할 수 있다.
XML, annotation, 자바 설정 클래스 등을 사용하여 빈을 정의하고 관리할 수 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZpzTN%2FbtsziPkuE67%2FlKTz2xI7NDkkVW8UHYZui0%2Fimg.png)