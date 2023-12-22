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

@Transactional을 선언하여 사용한다. 
클래스 또는 메서드 위에 @Transactional을 붙이면, 트랜잭션 기능이 적용된 프록시 객체가 생성되며, 트랜잭션 성공 여부에 따라 Commit 또는 Rollback 작업이 이루어진다.
또 다른 방법으로는 트랜잭션 스크립트 방식인데, 하나의 트랜잭션 안에서 동작해야 하는 코드를 한 군데 모아서 만드는 방식이다. 
이러한 방식 때문에 보통 트랜잭션마다 하나의 메서드로 구성된다.
메서드의 앞부분에서 DB를 연결하고 트랜잭션을 시작하는 코드가 필요하고, 이렇게 만들어진 
트랜잭션 안에서 DB를 액세스하는 코드와 그 결과를 가지고 비즈니스 로직을 적용하는 코드가 
뒤엉켜서 등장한다. 트랜잭션 스크립트 방식을 사용한다면 이런 코드들 때문에 어쩔 수 없는 중복 현상이 빈번하게 발생한다.

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