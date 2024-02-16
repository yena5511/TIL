## Annotation이란

사전상으로는 주석의 의미이지만 Java에서는 주석 이상의 기능을 가지고 있다
Annotation은 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종이다
소스코드에 추가하면 단순 주석의 기능을 하는 것이 아니라 특별한 기능을 사용할 수 있다

이런한 Annotation을 통하여 코드량이 감소하고 유지보수하기 쉬우며, 생산성이 증가된다

#### @Component

개발자가 생성한 class를 Spring의 Bean으로 등록할 때 사용한다.
spring은 해당 Annotation을 보고 Spring의 Bean으로 등록한다

```java
@Component(value="myman")
public class Man {
    public Man() {
        System.out.println("hi");
    }
}
```

#### @ComponentScan

Spring Framework는 @Component,  @Service, @Repository, @Controller, @Configuration 중 1개라도 등록된 클래스를 찾으면, Context에 bean으로 등록한다.
 @ComponentScan Annotation이 있는 클래스의 하위 Bean을 등록 될 클래스들을 스캔하여 Bean으로 등록해준다.

 #### @Bean

 @Bean은 개발자가 제어가 불가능한 외부 라이브러리와 같은 것들을 Bean으로 만들 때 사용한다.

 #### @Controller

 Spring에게 해당 class가 Controller의 역할을 한다고 명시하기 위해 사용한다.

 ```java
 @Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
}
 ```

#### @RestController

Spring에서 Controller 중 view로 응답하지 않는, Controller를 의미한다.
method의 반환 결과를 JSON향태로 반환하다.
이 Annotation이 적혀있는 COntroller의 method는 HTTPResponse로 바로 응답이 가능하다.
@ResposeBoby 역할을 자동적으로 해주는 Annotation이다.
@Controller + @ResponseBody를 사용하면 @ResponseBody를 모든 메소드에서 적용한다.

#### @Controller 와 @RestController 의 차이

- @Controller
    - API와 view를 동시에 사용하는 경우에 사용한다.
    - 대신 API서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환한다.
    - view(화면) return이 주목적이다.

- @RestController
    - view가 필요없는 API만 지원하는 서비스에서 사용한다
    - @RequestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정한다.
    - data(json, xml 등) return이 주목적이다.

즉, @RestController = @Controller + @ResponseBody 이다.

#### @RequestHeader

Request의 hearder 값을 가져올 수 있으며, 해당 Annotation을 쓴 메소드의 파라미터에 사용한다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(@RequestHeader(value="Accept-Language") String acceptLanguage) {
        //  GET method, /user 요청을 처리
    }
}
```

#### @Repository

DAO class에서 쓰인다.
DataBase에 접근하는 method를 가지고 있는 Class에서 쓰인다.

#### @RequestMapping

@RequestMapping(value=”“)와 같은 형태로 작성하며, 요청 들어온 URI의 요청과 Annotation value 값이 일치하면 해당 클래스나 메소드가 실행된다.
 Controller 객체 안의 메서드와 클래스에 적용 가능하며, 아래와 같이 사용한다.

 - class 단위에 사용하면 하위 메소드에 모두 적용
 - 메소드에 적용되면 해당 메소드에서 지정한 방식으러 URI를 처리한다.


```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }
    @RequestMapping(value = "/info", method = RequestMethod.GET)
    public String addUser(Model model) {
        //  GET method, /user/info 요청을 처리
    }
}
```

#### @RequestParam

URL에 전달되는 파라미터를 메소드의 인자와 매칭시켜, 파리미터를 받아서 처리할 수 있는 Annotation으로 아래와 같이 사용된다.
Json 형식의 Body를 MessageConverter를 통해 Java 객체로 변환시킨다

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(@RequestParam String nickname, @RequestParam(name="old") String age {
        // GET method, /user 요청을 처리
        // https://naver.com?nickname=dog&old=10
        String sub = nickname + "_" + age;
        ...
    }
}
```

#### @RequestBody

Body에 전달되는 데이터를 메소드의 인자와 매칭시켜, 데이터를 받아서 처리할 수 있는 Annotation으로 아래와 같이 사용한다.
클라이언트가 보내는 HTTP 요청 본문(JSON 및 XML 등)을 Java 오브젝트로 변환한다.

클라이언트가 body에 json or xml과 같은 형태로 값(부로 객체)를 전송하면, 해당 내용을 Java Object로 변환한다

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(@RequestBody User user) {
        //  POST method, /user 요청을 처리
        String sub_name = user.name;
        String sub_old = user.old;
    }
}
```

#### @ModelAttribute

클라이언트가 전송하는 HTTP parameter, Body 내용을 Setter 함수를 통해 1:1로 객체에 데이터를 연결(바인딩)한다.
RequestBody와 다르게 HTTP Body 내용은 multipart/form-data 형태를 요구한다.
 @RequestBody가 json을 받는 것과 달리 @ModelAttribute 의 경우에는 json을 받아 처리할 수 없다.

#### @ResponseBody

@ResponseBody은 메소드에서 리턴되는 값이 View로 출력되지 않고 HTTP Response Body에 직접 쓰여지게 됩니다. return 시에 json, xml과 같은 데이터를 return 한다

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    @ResponseBody
    public String getUser(@RequestParam String nickname, @RequestParam(name="old") String age {
        // GET method, /user 요청을 처리
        // https://naver.com?nickname=dog&old=10
        User user = new User();
        user.setName(nickname);
        user.setAge(age);
        return user;
    }
}
```

#### @Autowired

Sprin Framework에서 Bean 객체를 주입받기 위한 방법은 크게 3가지가 있다.
Bean을 주입받기 위하여 @Autowired를 사용한다.
Spring Framework가 Class를 보고 Type애 맞게 (Type을 먼저 확인 후, 없으면 Name 확인) Bean을 주입한다

- @Autowired
- 생성자 (@AllAragsConstructor 사용)
- setter

#### @GetMapping

RequestMapping(Method=RequestMethod.GET)과 똑같은 역할을 하며, 아래와 같이 사용한다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @GetMapping("/")
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
    
    ////////////////////////////////////
    // 위와 아래 메소드는 동일하게 동작합니다. //
    ////////////////////////////////////

    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
}
```

#### @PostMapping

RequestMapping(Method=RequestMethod.POST)과 똑같은 역할을 하며, 아래와 같이 사용한다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }

    ////////////////////////////////////
    // 위와 아래 메소드는 동일하게 동작합니다. //
    ////////////////////////////////////

    @PostMapping('/')
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }
}
```

#### @SpringBootTest

Spring Boot Test에 필요한 의존성을 제공한다.

```java
// DemoApplicationTests.java
package com.example.demo;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class DemoApplicationTests {

	@Test
	void contextLoads() {

	}

}
```

#### @Test

JUnit에서 테스트 할 대상을 표시한다. 

#### @Resource

@Autowired와 마찬가지로 Bean 객체를 주입해주는데 차이점은 Autowired는 타입으로, Resource는 이름으로 연결해준다.
Annotation 사용으로 인해 특정 Framework에 종속적인 어플리케이션을 구성하지 않기 위해서는 @Resource를 사용할 것을 권장한다.
@Resource를 사용하기 위해서는 class path 내에 jsr250-api.jar 파일을 추가해야 한다.
필드, 입력 파라미터가 한 개인 bean property setter method에 적용 가능하다.

#### @PathVariable

REST API에서 URL에 변수가 들어가는걸 실무에서 많이 볼 수 있다.
예를 들면, 아래 URL에서 `1234`, `4062464`이 @PathVariable로 처리해줄 수 있는 부분이다.

- http://localhost:8080/api/user/_1234_
- https://music.bugs.co.kr/album/_4062464_


Controller에서 아래와 같이 작성하면 간단하게 사용 가능하다.

1. @GetMapping(PostMapping, PutMapping 등 다 상관없음)에 {변수명}
2. 메소드 정의에서 위에 쓴 변수명을 그대로 @PathVariable("변수명") 
3. (Optional) Parameter명은 아무거나 상관없음(아래에서 String name도 OK, String employName도 OK)

```java
@RestControllerpublic class MemberController {     
    // 기본
    @GetMapping("/member/{name}")
    public String 
findByName(@PathVariable("name") String name ) { 
           return "Name: " + name; 
              }   
              
    // 여러 개 
    @GetMapping("/member/{id}/{name}")
    public String 
findByNameAndId(@PathVariable("id") String id, @PathVariable("name") String name) {    
                return "ID: " + id + ", name: " + name;   
                 }    
                            }

```

#### @Transactional

- 스프링 프레임워크에서 특정 메서드 또는 클래스에서 수행되는 '트랜잭션'과 관련되어 관리를 위해서 사용되는 어노테이션이다.
- 메서드 또는 클래스에 해당 어노테이션을 선언하면 코드 실행 중에 트랜잭션에 오류가 발생되면 트랜잭션이 '롤백'되고 변경 사항이 모두 취소된다.
- 해당 어노테이션은 ‘org.springframework.boot:spring-boot-starter-web' 를 추가하면 사용 가능하다.

- 트랜잭션
    - 데이터베이스 작업을 원자적으로 처리 하기 위한 메커니즘을 의미한다.
    - 여러 개의 데이터 베이스 작업을 하나의 논리적 단위로 묶어서 실행하고 모즌 작업이 성공적으로 완료하면 커밋하고너 실패할 경우 롤백하는 기능을 제공한다.

- 원자적 처리
    - 트랜잭션 내의 모든 데이터 베이스 작업이 모두 성공하거나 모두 실패할 때까지 어떤 작업도 부분적으로 적용되지 않는 다는 것을 의미한다.
    - 트랜잭션은 모든 작업이 완전히 수행되기 전까지는 어떤 변화도 발생하지 않도록 보장한다.

- 트랜잭션 이점
    - 자동으로 트랜잭션을 시작하고 커밋 또는 롤백할 수 있다.
    - 트랜잭션 경계를 명시적으로 설정할 필요가 없다.
    - 예외 발생 시 롤백이 처리된다.

```java
@Service
@RequiredArgsConstructor
public class OrderService {
    private final PayService payService;
    private final OrderRepository orderRepository;

    @Transactional(readOnly = true)
    public Mcorder findById(long id) {
        final Optional<Mcorder> order = orderRepository.findById(id);
        order.orElseThrow(() -> new OrderNotFoundException(id));
        return order.get();
    }

    @Transactional
    public Mcorder create(Mcorder order) {
        Mcorder order = orderRepository.save(order);
        payService.invoke(order.payment);
        return order;
    }
}
```

#### @Qualifier 

bean을 설정할 떄 <context:annotation-config/>를 사용함으로써 굳이 bean 내 그안에 <contstructor-arg>나 <property>태그를 추가하지 않아도 스프링의 @Autowired 어노테이션이 적용된 생성자, 필드, 메서드에 대한 의존 자동 주입을 처리한다.

하지만 동일한 타입을 가진 bean 객체가 두 개가 있다면?
- 스트링이 어떤 빈을 주인해야 할 지 알 수 없어서 스프링 컨테이너를 초기화하는 과정에서 Exception을 발생시킨다.
    - @Autowired의 주인 ㅈ대상이 한 개여야 하는데 실제로는 두 개 이상의 빈이 존재해 주입할 때 사용할 객체를 선택할 수 없기 떄문이다.
    - 단, @Autowired가적용된 필드나 설설정 메서드의 property이름과 같은 이름을 가진 빈 객체가 존재할 경유에는 이름이 같은 빈 객체를 주입받는다.

이런 문제를 해결하기 위해 @Qualifier 사용

- @Qualifier 어노테이션
    - 사용할 의존 객체를 선택할 수 있도록 해준다.
    1. 설정에서 bean의 한정자 값을 설정한다.
    2. @Autowired어노테이션이 적용된 주입 대상에 @Qualifier 어노테이션을 설정한다.
        - 이때 @Qualifier 의 값으로 앞서 설정한 한정자를 사용한다.

```java
pubic class MemberDao{  
    @Autowired  @Qualifier("m1")
    private Member member;       

    public void setMember(Member member){      
        this.member = member;  
    }
}
```
위와같이 @Autowired 어노테이션을 적용한 곳에 @Qualifier 어노테이션을 적용해준다.

- 주의할 점
    - @Qualifier에 지정한 한정자 값을 갖는 bean 객체가 존재하지 않으면 Exception이 발생한다

#### @Primary

같은 Type의 Bean이 여러개가 필요한 경우도 있다. 
그럴 때 어노테이션을 붙혀서 우선순위를 지정할 때 사용한다. 
특정 빈을 우선적으로 주입하고 싶은 경우 해당 어노테이션을 붙여준다.

```java
@Component
@Primary
public class FixDiscountPolicy implement DiscountPolicy{
    @Override
    public int discount(int cost){ return cost - 1000;}
}
@Component
public class RateDiscountPolicy implement DiscountPolicy{
    @Override
    public int discount(int cost){ return cost - (cost * 1/10);}
}


@Autowierd
private DiscountPolicy policy; // FixDiscountPolicy 메핑
```

※ 만약 Primary와 Qualifier를 동시에 사용할 경우 Qualifier가 우선권을 가진다.

#### @jsonIgnore

직렬화 역직렬화에 사용되는 논리적 프로퍼터 값을 무시 할 때 사용된다.

```java
@jsonIgnore
public String myName;


또한 게터 세터 메서드에서도 사용 가능하다.

@JsonIgnore
public String getmyName(){
	return this.myName;
}

@JsonIgnore
public void setmyName(String myName){
	return this.myName = myName;
}

```

myName필드가 역직렬화할 때 서버는 Unrecognizef find "myNama"와 같은 에러를 던질 것 이다. 작렬화 할 땐  myName 필드가 Json에 담기지 않습니다.

@JsonIgnore어노테이션은 @JsonProperty와 함께 사용될 수 있다. 

```java
@JsonIgnore
@JsonProperty("Name")
private String myName;
```

myName 프로퍼터에  @JsonProperty("myName")를 추가함으로서 에러를 던지지 않으면서
JSON의 직렬, 역직렬화 할때 myName필드를 무시할 수 있다.

또한  @JsonIgnore의 속성값으로(false)를 줄 수 있다.

#### @JsonIgnoreProperties

@JsonIgnoreProperties어노테이션은 단위 레벨에 사용되며 지정된 필드값의 JSON직력, 역렬화를 무시할 수 있다.

```java
@JsonIgnoreProperties({ "bookName", "bookCategory" })
public class Book {public class Book {
	@JsonProperty("bookId")
	private String id;private String id;
    
	@JsonProperty("bookCategory")
	private String category;
```

위 코드에서 bookname, bookcategory필드는@JsonIgnoreProperties에 지정된어 JSON 직렬, 역직렬화에 담기지 않을 것 이다.
만약 book클래스 네에 bookid같은 필드가 존재하고, @JsonIgnoreProperties이 적용된다면 book클래스의 모든 필드값은 JSON에 담기지 않을 것이다.

이 말은 즉 @JsonIgnore, @JsonIgnoreProperties은 JSON 직렬, 역직렬화를 무시할때 사용 된다.

@JsonIgnoreProperties 어노테이션은
allowGetters, allowSetters, ignoreUnknown, value 속성을 가지고 있다.

```java
@JsonIgnoreProperties
(
value={"bookName","bookCategory"}, allowGetters=true
)
```
는 특정 필드가 직렬화는 허용하지만 역직렬화는 허용하지 않게 해주는 어노테이션이다.

```java
@JsonIgnoreProperties
(
value={"bookName","bookCategory"}, allowSettersa=true
)
``````
는 allowGetters와 반대로 역직렬화는 허용하나 직렬화는 허용하지 않게 해주는 어노테이션 이다.

```java
@JsonIgnoreProperties(ignoreUnknown = true)
```
현재 진행중인 프로젝트에서 사용되고 있으며
아마 가장 많이 쓰이는 어노테이션이라 생각되는 속성이다.

```java
@JsonIgnoreProperties(ignoreUnknown = true)
```
를 사용하게 되면 역직렬화,
JSON 데이터가 가진 프로퍼티 중에
자바 class의 vo 프로퍼티에 값이 없는경우
에러를 던지지 않고 무시된다.

```java
##Class Person ###

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;
@Data	//lombok
@JsonIgnoreProperties(ignoreUnknown = true)
public class person{
	private String name;
    	private int age;
}


##JSON##

{
	"name" : "han",
    	"age"  : "29",
        "language" : "korean",
        "gender" : "male"
}
```
위의 Person class는 name과 age 프로퍼티를 가지고 있다.
JSON 데이터는 Person class 존재하지 않는 추가적인 필드가 존재한다.
Json 데이터의 language, gender는 Person 클래스의 논리적인 필드값에 대응하지 않는다.
하지만 @JsonIgnoreProperties 의 ignoreUnknown =true 속성을 사용함 으로써 역직렬화(클라이언트 -> 서버)에 예외를 받지 않을것 이다.

#### @JsonIgnoreType

@JsonIgnoreType 어노테이션은 클래스 레벨에 사용되며 클래스내에 존재하는 모든 필드들이 JSON 직렬, 역직렬화에 무시될 것이다.
(즉 클래스 자체를 JSON 데이터 맵핑에 사용불가)
마찬가지로 @JsonIgnoreType(false)와 같이 boolean 속성을가지고 있으며
활성/ 비활성화 할 수 있다.
//default 값 = true

```java
@JsonIgnoreType
class Address{
	private String address;
}

public class Writer {
    @JsonProperty("writerAddress")
    private Address address;
}
```
프로퍼티 writerAddress는 Writer 클래스의 직렬, 역직렬화에 무시될것 한다.
만약 Address 클래스에
@JsonIgnore(false)를 붙혀주시면 활성/비활성화를 컨트롤할 수 있다.

#### @CookieValue

쿠키 값을 파라미터로 전달 받을 수 있는 방법으로, 해당 쿠키가 존재하지 않으면 500에러를 발생시킨다.

```java
public String view(@CookieValue(value="auth") String auth) {
	.....
}
```

#### @CrossOrigin

CORS보안상의 문제로 브라우저에서 리소스를 현재 origin에서 다른 곳으로 AJAX요청을 방지하기 위해 사용한다.
@RequestMapping이 있는 곳에 사용하면 해당 요청은 타 도메인에서 온 ajax요청을 처리한다.

```java
@RestController
@RequestMapping("/account")
public class AccountController{
	
    @CrossOrigin
    @RequestMapping("/{id}")
    public Account retrieve(@PathVariable Long id){
    	// ...
    }
    
    
    @RequestMapping(method = RequestMethod.DELETE, value = "/{id}")
    public void remove(@PathVariable Long id){
    // ...
    }
}
```

#### @ConfigurationProperites

Spring boot 는 configuration 몇몇 속성을 *.properties 혹은 *.yaml 에서 쉽게 가져오는 방법을 제공하기 시작했다.

application.properties 나 application.yml 의 속성을 가져올 때 기존엔 다음과 같이 사용했을 것이다.
```java
@Value( "${jdbc.url}" )
private String jdbcUrl;
```
하지만, 이젠 이를 더 객체지향적으로 다룰 수 있으며, 덕분에 테스트에 사용하기도 편리해졌다.

@ConfigurationProperties는 프로퍼티에 있는 값을 클래스로 바인딩하기 위해 사용되는 어노테이션이다. @ConfigurationProperties는 값의 바인딩을 위해 Setter를 필요로 하며 생성자로 바인딩하기 위해서는 @ConstructorBinding을 붙여주어야 한다. 

```yaml
mail:
	hostname: host@mail.com
	port: 9000
	mail: mailer@mail.com
```
이 속성을 다음과 같이 가져와서 사용할 수 있다.
```java
@Data
@Configuration
@ConfigurationProperties(prefix = "mail")
public class ConfigProperties {
    
    private String hostName;
    private int port;
    private String from;

}
```

#### @Valid

- 빈 검증기를 이용해 객체의 제약 조건을 검증하도록 지시하는 어노테이션이다.
- SFR 표준의 빈 검증 기술의 특징은 객체의 필드에 달린 어노테이션으로 편리하게 검증을 한다는 것이다.
- Spring에서는 일종의 어댑터인 LocalValidatorFactoryBean가 제약 조건 검증을 처리한다. 
이를 이용하려면 LocalValidatorFactoryBean을 빈으로 등록해야 하는데, SpringBoot에서는 아래의 의존성만 추가해주면 해당 기능들이 자동 설정된다.

|annotation|설명|
|:---|:---|
|@Null|Null만 입력 가능|
|@NotNull|Null 불가|
|@NotEmpty|Null, 빈 문자열 불가|
|@NotBlank|Null, 빈 문자열, 스페이스만 있는 문자열 불가|
|@Size(min=,max=) | 값이 min과 max사이에 해당하는가? (CharSequence, Collection, Map, Array에 해당)|
|@Pattern(regex=) |정규식을 만족하는가?|
|@Positive |양수만 가능|
|@PositiveOrZero|양수와 0만 가능|
|@Negative|음수만 가능|
|@NegativeOrZero|음수와 0만 가능|
|@Digits(integer=, fraction = ) |대상 수가 지정된 정수와 소수 자리 수 보다 작은가?|
|@Max(숫자)|값이 Max보다 큰지 확인|
|@Min(숫자) |값이 Min보다 작은지 확인|
|@Email |이메일 형식만 가능|
|@AssertTrue |true 인가?|
|@AssertFalse |false 인가?|

```java
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-validation'
```

```java
@Getter
@RequiredArgsConstructor
public class AddUserRequest {

	@Email
	private final String email;

	@NotBlank
	private final String pw;

	@NotNull // null이 아님을 확인
	private final UserRole userRole;

	@Min(12) // 해당 값의 최솟값을 지정
	private final int age;

}
```

다음과 같이 컨트롤러의 메서드에 @Valid를 붙여주면 유효성 검증이 진행된다.

```java
@PostMapping("/user/add") 
public ResponseEntity<Void> addUser(@RequestBody @Valid AddUserRequest addUserRequest) {
      ...
}
```

#### @PathVariable

- 경로 변수를 표시하기 위해 메서드에 매개변수에 사용된다.
- 경로 변수는 중괄호 {id}로 둘러싸인 값을 나타낸다.
- URL 경로에서 변수 값을 추출하여 매개변수에 할당한다.
- 기본적으로 경로 변수를 반드시 값을 가져야 하며, 값이 없는 경우 404오류가 발생
- 주로 상세 조회, 삭제와 같은 작업에서 식별자로 사용
- 여러 개의 PathVariable을 동시에 사용할 수 있다.

```java
 @DeleteMapping("/{comment_id}")
    public ResponseEntity<String> delete(@PathVariable Long comment_id){
        commentService.deleteComment(comment_id);
        return ResponseEntity.status(HttpStatus.OK).body("댓글 삭제가 완료되었습니다.");
    }
````
@PathVariable의 이름과 url의 괄호 안의 이름이 같은 경우 data에 해당 값을 저장한다.
