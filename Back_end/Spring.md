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

    - `test.mv.db` 파일 확인
    - `jdbc:h2:tcp://localhost/~/test` 

    #### 테이블 생성하기

    테이블 관리를 위해 프로젝트 루트에 `sql/ddl.sql`파일을 생성

```
    create table member
(
    id  bigint generated by default as identity,
    name varchar(255)
    primary key(id)
);
```

#### H2 데이터베이스가 정상 생성되지 않을 때

1. H2데이터 베이스를 종료하고, 다시 시작한다.
2. 웹 브라우저가 자동 실행되면 주소창에 다음과 같이 되어있다.
3. 다음과 같이 앞 부분과 `100.1.2.3` -> `localhost`로 변경하고 Enter를 입력한다. 나머지 부분도 절대 변경하면 안 된다. (특히 뒤에 세션 부분이 변경되면 안된다.) 
4. 데이터베이스 파일을 생성하면 `jdbc:h2:~/test` 데이터 베이스가 생성된다.

## 순수 JDBC

#### 환경설정
*build.gradle 파일에 jdbc,h2 데이터베이스 환경 라이브러리 추가*

```
	implementation'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'com.h2database:h2'
```

#### jdbc 리포지토리 구현

주의 이렇게 JDBC API로 직접 코딩하는 것은 15년 전 이야기이다. 따라서 고대 개발자들이 이렇게 고생하고 살았구나 생각하고, 정신건강을 위해 참고만 하고 넘어가자




## 스프링 통합 테스트

스프링 컨테이너와 DB까지 연결한 통합 테스트를 진행해보자.

*회원 서비스 스프링 통합 테스트*
```
package exam.Spring.service;

import exam.Spring.domain.Member;
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;


import static org.junit.jupiter.api.Assertions.assertThrows;
@SpringBootTest
@Transactional
 class MemberServiceIntegrationTest {
   @Autowired MemberService memberService;
    @Autowired MemberRepository memberRepository ;

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

    }

    @Test
    void findMembers() {


    }

    @Test
    void findOne() {
    }
}
```
- @SpringBootTest: 스프링 컨테이너와 테스트를 함께 실행한다.

- @Transactional: 테스트 케이스에 이 애노테이션이 있으면 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.


## 스프링 JdbcTemplate

- 순수 Jdbc의 동일한 환경설정을 하면 된다.
- 스프링 JdbcTemplate과 MyBatis 같이 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야한다.

*스프링 JdbcTemplate 회원 리포지토리*
```
package exam.Spring.repository;

import exam.Spring.domain.Member;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;

import javax.sql.DataSource;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.*;

public class JdbcTemplateMemberRepository implements MemberRepository {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public JdbcTemplateMemberRepository(DataSource dataSource){
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }
    @Override
    public Member save(Member member){
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
        jdbcInsert.withCatalogName("member").usingGeneratedKeyColumns("id");

        Map<String, Object> parameters = new HashMap<>();
        parameters.put("name", member.getName());

        Number Key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
        member.setId(Key.longValue());
        return member;
    }

    @Override
    public Optional<Member> findByID(Long id){
        List<Member> result =  jdbcTemplate.query("select * from member where id = ?", memberRowMapper());
        return result.stream().findAny();
    }
    @Override
    public Optional<Member> findByName(String name){
        List<Member> result =  jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll(){
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getNString("name"));
            return member;
        };
    }


}
```

*JdbcTemplate을 사용하도록 스프링 설정 변경*
```
package exam.Spring;

import exam.Spring.repository.JdbcMemberRepository;
import exam.Spring.repository.JdbcTemplateMemberRepository;
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;
import exam.Spring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class SpringConfig {
    private final DataSource dataSource;

    public SpringConfig(DataSource dataSource){
        this.dataSource = dataSource;
    }


    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
     //   return new MemoryMemberRepository();
        return new JdbcTemplateMemberRepository(dataSource);
    }

}
```

## JPA
- JPA는 기존의 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다.
- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.
- JPA를 사용하면 개발 생산성을 크게 높일 수 있다.

*build.gradle 파일 JPA, h2 데이터 베이스 관련 라이브러리 추가*
```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	//implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	runtimeOnly 'com.h2database:h2'
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

```
`spring-boot-starter-date-jpa`는 jdbc관련 라이브러리를 포함한다. 따라서 jdbc는 제거해도 된다.

*스프링 부트에 JPA 설정 추가*
`resources/application.properties`
```
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none
```
- `show-sql` : JPA가 생성하는 SQL을 출력한다.
- `ddl-auto`: JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 'none'를 사용하면 해당 기능을 끈다.
    - `create`를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다 -> 해보자.

*JPA 엔티티 매핑
```
package exam.Spring.domain;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Member {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
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

*JPA 회원 리포지토리*
```
package exam.Spring.repository;

import exam.Spring.domain.Member;

import javax.persistence.EntityManager;
import java.util.List;
import java.util.Optional;

public class JpaMemberRepository implements MemberRepository{

    private final EntityManager em;

    public JpaMemberRepository(EntityManager em){
        this.em = em;
    }
    @Override
    public Member save(Member member){
        em.persist(member);
        return member;
    }
    @Override
    public Optional<Member> findByID(Long id){
        Member member = em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {

        List<Member> result =  em.createQuery("select m from Member m Where m.name = :name", Member.class )
                .setParameter("name", name)
                .getResultList();
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }


}
```


*서비스 계층에 트랜잭션 추가*
```
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.Optional;

@Transactional
public class MemberService {
}
```

*JPA를 사용하도록 스프링 설정 변경*

```
package exam.Spring;

//import exam.Spring.repository.JdbcMemberRepository;
import exam.Spring.repository.JdbcTemplateMemberRepository;
import exam.Spring.repository.JpaMemberRepository;
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;
import exam.Spring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.sql.DataSource;

@Configuration
public class SpringConfig {
    private EntityManager em;

    @Autowired
    public SpringConfig(EntityManager em){
        this.em = em;
    }

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
     //   return new MemoryMemberRepository();
     //   return new JdbcTemplateMemberRepository(dataSource);
        return new JpaMemberRepository(em);
    }
}
```

## 스프링 데이터 JPA

스프링 부트와 JPA만 사용해도 개발 생산성이 정말 많이 증가하고, 개발해야할 코드도 확연히 줄어듭니다. 
여기에 스프링 데이터 JPA를 사용하면, 기존의 한계를 넘어 마치 마법처럼, 리포지토리에 구현 클래스 없이
인터페이스 만으로 개발을 완료할 수 있습니다. 그리고 반복 개발해온 기본 CRUD 기능도 스프링 데이터
JPA가 모두 제공합니다.
스프링 부트와 JPA라는 기반 위에, 스프링 데이터 JPA라는 환상적인 프레임워크를 더하면 개발이 정말
즐거워집니다. 지금까지 조금이라도 단순하고 반복이라 생각했던 개발 코드들이 확연하게 줄어듭니다. 
따라서 개발자는 핵심 비즈니스 로직을 개발하는데, 집중할 수 있습니다.
실무에서 관계형 데이터베이스를 사용한다면 스프링 데이터 JPA는 이제 선택이 아니라 필수 입니다.

주의: *스프링 데이터 JPA는 JPA를 편리하게 사용하도록 도와주는 기술입니다. 따라서 JPA를 먼저 학습한
후에 스프링 데이터 JPA를 학습해야 합니다.*

- 앞의 JPA 설정을 그대로 사용한다

*스프링 데이터 JPA 회원 리포지토리*
```
package exam.Spring.repository;

import exam.Spring.domain.Member;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {

    @Override
    Optional<Member> findByName(String name);

}
```


*스프링 데이터 JPA 회원 리포지토리를 사용하도록 스프링 설정 변경 *
```
package exam.Spring;

//import exam.Spring.repository.JdbcMemberRepository;
import exam.Spring.repository.JdbcTemplateMemberRepository;
import exam.Spring.repository.JpaMemberRepository;
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;
import exam.Spring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.sql.DataSource;

@Configuration
public class SpringConfig {


    private final MemberRepository memberRepository;

    @Autowired
    public SpringConfig(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository);
    }

//    @Bean
 //   public MemberRepository memberRepository(){
     //   return new MemoryMemberRepository();
     //   return new JdbcTemplateMemberRepository(dataSource);
     //   return new JpaMemberRepository(em);
//    }
}
```

- 스프링 데이터 JPA가 `springDataMemberRepository`를 스프링 빈으로 자동 등록해준다.

*스프링 데이터 JPA 제공 기능*
- 인터페이스를 통한 기본적인 CRUD
- `findByName(), findByEmail()` 처럼 메소드 이름만으로 조회 기능 제공

참고: 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 된다. Querydsl을 사용하면 쿼리도 자바코드로 안전하게 작성할 수 있고, 동적쿼리도 편리하게 작성할 수 있다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, 앞서 학습한 스프링 JdbcTemplate를 사용하면 된다.

## AOP

#### AOP가 필요한 상황
- 모든 메소드의 호출 시간을 측정하고 싶다면?
- 공통 관심사항(cross-cutting concern) vs 핵심 관심 사항(core concern)
- 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

*MemberService 회원 조회 시간 측정 추가*
```
package exam.Spring.service;

import exam.Spring.domain.Member;
import exam.Spring.repository.MemberRepository;
import exam.Spring.repository.MemoryMemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;
import java.util.Optional;

@Transactional
public class MemberService {

    private final MemberRepository memberRepository;
    public MemberService(MemberRepository memberRepository) {

        this.memberRepository = memberRepository;
    }
    /**
     * 회원가입
     */
    public Long join(Member member) {

        long start = System.currentTimeMillis();

        try{

        validateDuplicateMember(member);//중복 회원 검증
        memberRepository.save(member);
        return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join = " + timeMs + "ms");
            }
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
        long start = System.currentTimeMillis();
        try {
            return memberRepository.findAll();
        } finally {
           long finish =  System.currentTimeMillis();
           long timeMs = finish - start;
            System.out.println("findMembers " + timeMs + "ms");
        }

    }
}
```

문제
- 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.
- 시간을 측정하는 로직은 공통 관심 사항이다.
- 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.
- 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야 한다.


## AOP 적용
- AOP: Aspect Oriented Programming
- 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리


*시간 측정 AOP 등록*
```
package AOP;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* exam.Spring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try {
            return joinPoint.proceed();
        }finally {
             long finish = System.currentTimeMillis();
             long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");

        }


    }
}
```

*해결*
- 회원가입, 회원 조회등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리한다.
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들었다.
- 핵심 관심 사항을 깔끔하게 유지할 수 있다.
- 변경이 필요하면 이 로직만 변경하면 된다.
- 원하는 적용 대상을 선택할 수 있다.