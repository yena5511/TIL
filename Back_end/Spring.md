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


```public class MemberForm {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name){
        this.name = name;
    }
}
```


```
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
    @PostMapping("/members/new")
    public String create(MemberForm form){
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        return "redirect:/";
    }

}
```


## 회원 웹 기능 - 조회
*회원 컨트롤러에서 조회 기능*
```
    @GetMapping("/members")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return "members/memberList";
    }
    ```


*회원 리스프 HTML*

```
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>

<div class="Container">
    <div>
        <table>
            <thead>
            <tr>
                <th>#</th>
                <th>이름</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each = "member : ${members}">
                <td th:text = "${member.id}></td>
                <td th:text = "${member.name}></td>
"
            </tr>
            </tbody>
        </table>
    </div>
</div>
</body>
</html>
```




## H2 데이터베이스 설치
개발이나 테스트 용도로 가볍고 편리한 DB, 웹 화면 제공

- 다운로드 및 설치
- h2 데이터베이스 버전은 스프링 부트버전에 맞춘다.
- 권한 주기: `chmod 755 h2.sh`
- 실행: `./h2.sh`
- 데이터베이스 파일 생성 방법
    - `jdbc:h2:~/test`(최초 한번)
    - `/test.mv.db`파일 생성 확인
    - 이후부터는 `jdbc:h2:tcp://localhost/~/test`이렇게 접속

    #### 테이블 생성하기

    테이블 관리를 위해 프로젝트 루트에 `sql/ddl.sql`파일을 생성