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


## View 환경설정
#### Welcome Page 만들기
```
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charst-8" />

</head>
<body>
Hello
<a href="/hello">hello</a>
</body>
</html>
```


코드 실행
:http://localhost:8080



#### thymeleaf 템플릿 엔진
```
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```


코드실행
:http://localhost:8080/hello

참고: `spring-boot-devtools` 라이브러리를 추가하면, `html` 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다.



## 빌드하고 실행하기

1.`./gradlew build`<br>
2.`cd build/libs`<br>
3.`java -jar hello-spring-0.0.1-SNAPSHOT.jar`<br>
4.실행 확인



## 정적 컨텐츠


```<!DOCTYPE HTML>
<html>
<head>
    <title>static content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

코드실행
:http://localhost:8080/hello-static.html


## MVC와 템플릿 엔진
 MVC : Model, View, Controller
 #### *Controller*
```
@GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-template";
    }
```


 #### *View*
```
<html xmlns:th="http://.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```


코드 실행
:http://localhost:8080/hello-mvc?name=spring

## API

#### @ResponseBody 문자변환
```
@GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name){
        return "hello" + name;
    }
```

코드 실행
:http://localhost:8080/hello-string?name=spring


#### @ResponseBody 객체 변환
```
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name){
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello{
        private String name;

        public String getName(){
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
```
코드 실행
:http://localhost:8080/hello-api?name=spring


- @ResponseBody를 사용 
    - HTTP의 BODY에 문자 내용을 직접 반환
    - `viewResolver` 대신에 `HttpMessageConverter`가 동작
    - 기본 문자처리 : `StringHttMessageConverter`
    - 기본 객체처리:
    `MappingJackson2HttpMessageConverter`
    - byte 처리 등등 기타 여러 `HttpMessageConverter`가 기본으로 등록되어 있음

    
참고: 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 들을 조합해서 `HttpMessageConverter`가 선택된다.



## 비지니스 요구사항 정리


- 데이터: 회원 ID, 이름
- 기능 : 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상화 시나리오)

*일반적인 웹 애플리케이션 계층 구조*

![Alt text](https://velog.velcdn.com/images/sloools/post/0bfa8372-ca51-4b20-aec7-d94fb465a4c0/image.png)

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스 : 핵심 비지니스 로직 구현
-  리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 : 비지니스 도메인 객체 예)회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

*클래스 의존관계*



- 아직 데이터 저장소가 선정되지 않아서, 우선 인터페이스로 구현 클래스를 변경할 수 있게 설계
- 데이터 저장소는 RDB, NoSQL 등등 다양한 저장소를 고민중인 상황으로 가정
- 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용


## 회원 도메인과 리포지토리 만들기

*회원 객체*
```
public class Member {


    private Long id;
    private String name;

    public Long getId(){
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }


    public void setName(String name) {
        this.name = name;
    }
}
```

*회원 리포지토리 인터페이스*
```
public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findByID(Long id);
    Optional<Member> findByName(String name);
   List<Member> findAll();

}
```


*회원 리포지토리 메모리 구현체*

```
import java.util.*;

public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member > store = new HashMap<>();
    private static long sequence = 0L;
    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;

    }

    @Override
    public Optional<Member> findByID(Long id) {
        return Optional.ofNullable(store.get(id));

    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>((store.values()));
    }

    public void clearStore() {
        store.clear();
    }
}
```


## 회원 리포지토리 테스트 케이스 작성

개발한 기능을 실행해서 테스트 할 때 자바의 main 메소드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다. 이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는 단점이 있다. 자바는 JUnit이라는 프레임워크로 테스트를 실행하여 이러한 문제를 해결한다.


*회원 리포지토리 메모리 구현체*
```
import exam.Spring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.Optional;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach(){
        repository.clearStore();
    }


    @Test
    public  void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

       Member result  =  repository.findByID(member.getId()).get();
        assertThat(member).isEqualTo(result);

    }

    @Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result =  repository.findByName("spring1").get();

        assertThat(result).isEqualTo(member1);
    }
     @Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

         Member member2 = new Member();
         member2.setName("spring2");
         repository.save(member2);

         List<Member> result = repository.findAll();

         assertThat(result.size()).isEqualTo(2);
     }

}
```

## 회원 서비스 개발

```
import exam.Spring.domain.Member;
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /**
     * 회원가입
     */
    public Long join(Member member) {
        validateDuplicateMember(member);//중복 회원 검증
        memberRepository.save(member);
        return member.getId();
    }
    public void validateDuplicateMember(Member member){
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });

    }


    /**
     * 전체회원 조회
     */
    public List<Member> findMembers(){
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId){
        return memberRepository.findByID(memberId);
    }

}
```


## 회원 서비스 테스트

```
class MemberServiceTest {

    MemberService memberService;
    MemoryMemberRepository memberRepository ;

    @BeforeEach
    public void beforEach(){
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach(){
        memberRepository.clearStore();
    }

    @Test
    void 회원가입() {
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        org.assertj.core.api.Assertions.assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외(){
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
        org.assertj.core.api.Assertions.assertThat(e. getMessage()).isEqualTo(("이미 존재하는 회원입니다."));



//        try {
//            memberService.join(member2);
//            fail();
//        } catch (IllegalStateException e){
//            org.assertj.core.api.Assertions.assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
//
//        }

        //then
    }

    @Test
    void findMembers() {


    }

    @Test
    void findOne() {
    }

}
```


## 컨포넌트 스캔과 자동 의존관계 설정

#### 스프링 빈을 등록하고, 의존관계 설정하기
회원 컨트롤러가 회원서비스와 회원 리포지토리를 사용할 수 있게 의존관계를 준비하자.

*회원 컨트롤러에 의존관계 추가*
```
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService){
        this.memberService = memberService;
    }

}
```

- 생성자에 '@Autowired'가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를 외부에서 넣어주는 것을 DI, 의존성 주입이라 한다.
- 이전 테스트에서는 개발자가 직접 주입했고, 여기서는 @Autowired에 의해 스프링이 주입해준다.


*스프링 빈을 등록하는 2가지 방법*
- 컴포넌트 스캔과 자동 의존관계 설정
- 자바 코드로 직접 스프링 빈 등록하기


#### 컴포넌트 스캔과 자동 의존관계 설정
- '@Component' 에노테이션이 있으면 스프링 빈으로 자동 등록된다.
- '@Controller'컨트롤러가 스프링 빈으로 자동 등록된 이우도 스캔 때문이다.
- '@Component'를 포함하는 다음 에노테이션도 스프링 빈으로 자동 등록된다.
    - '@Controller'
    - '@Service'
    - '@Repository'


*회원 서비스 스프링 빈 등록*

```
@Service
public class MemberService {

    private final MemberRepository memberRepository;
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

*회원 리포지토리 스프링 빈 등록*

```
@Repository
public class MemoryMemberRepository implements MemberRepository
```

참고: 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다(유일하게 하나만 등록해서 공유한다)따라서 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.


## 자바 코드로 직접 스프링 빈 등록하기

- 회원 서비스와 회원 리포지토리의 @Service, @Repository, @Autowired 에노테이션을 제거하고 진행한다.

```
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;
import exam.Spring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }


}
```

*여기서는 향후 메모리 리포지토리를 다른 리포지토리로 변경할 예정이므로, 컴포넌트 스캔 방식 대신에 자바 코드로 스프링 빈을 설정하뎄다.*
- 참고 : XML로 설정하는 방식도 있지만 최근에는 잘 사용하지 않으므로 생략한다.
- 참고: DI에는 필드주입, setter 주입, 생산자 주입 이렇게 3가지 방법이 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.
- 참고: 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다. 그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.
- 주의: 'Autowired'를 통한 DI는 'helloconroller', MemberService'등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.
- 스프링 컨테이너, DI 관련된 자세한 내용은 스프링 핵심 원리 관리에서 설명한다.


## 회원 웹 기능 - 홈 화면 추가

*홈 컨트롤러 추가*
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(){
        return "home";
    }

}
````

*회원 관리용
```
<!DOCTYPE HTML>
<html xmlns:th="http://ww.thymeleaf.org">
<body>

<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>


</div>
</body>
</html>
```

코드 실행
:http://localhost:8080/


## 회원 웹 기능 - 등록

#### 회원 등록 폼 개발

*회원 등록 폼 컨트롤러*
```
import exam.Spring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService){
        this.memberService = memberService;
    }

    @GetMapping("/members/new")
    public String createForm(){
        return "member/createMemberForm";

    }

}
```


*회원 등록 폼 HTML
```
<!DOCTYPE HTML>

<html xmlns:th="http://www.thymeleaf.org">
<body>

<div class = "container">

    <form action="/members/new" method="post">
        <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" name = "name" placeholder="이름을 입력하세요.">
        </div>
        <button type="submit">등록</button>
    </form>

</div>
</body>
</html>
```



## 회원 웹 기능 - 조회

