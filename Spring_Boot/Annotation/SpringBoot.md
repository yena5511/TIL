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

 - class 단위에 사용하묜 하위 메소드에 모두 적용
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
