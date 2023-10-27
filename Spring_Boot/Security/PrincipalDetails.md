## PrincipalDetails

- 스프링 시큐리티는 /login 주소 요청이 오면 해당 요청을 낚아채서 로그인을 진행시킬 수 있다.
=> WebSecurityConfigurerAdapter를 상속받아 configure을 override 한후 다음과 같이 작성한다.

```java
@EnableWebSecurity // 해당 파일로 시큐리티를 활성화
@Configuration // IoC
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   @Override
   protected void configure(HttpSecurity http) throws Exception {
      // super 삭제 - 기존 시큐리티가 가지고 있는 기능이 다 비활성화됨
      // super.configure(http);
      http.authorizeRequests()
         .antMatchers("/","/user/**","/image/**","/subscribe/**","/comment/**","/api/**")
         .authenticated()
         .anyRequest().permitAll()
         .and()
         .formLogin()
         .loginPage("/auth/signin") // GET
         .loginProcessingUrl("/auth/signin") // POST -> 스프링 시큐리티가 로그인 프로세스 진행
         .defaultSuccessUrl("/");
   }
}

```

- antMatchers("주소).authenticated(): 해당 주소들을 로그인 하지 않은 상태로 접근 불가능
- anyRequest().permitAll(): 나머지 주소들을 접근 가을
- formLogin(): 스프랑 시큐리티 인증 실행
- loinPage("/auth/signin"):  "/auth/signin" 따로 만든 로그인 페이지로 가는 주소
- loginProcessingUrl("/auth/signin"): POST 방식으로 접근하면 로그인 프로세스가 진행
- defaultSuccessUrl("/"): 로그인 완료후 이동되는 주소

"/auth/signin" 주소로 POST 방식의 데이터를 보내면 로그인 프로세스가 진행되므로 따로 컨트롤러를 만들 필요가 없다.

```java
<form class="login__input" action="/auth/signin" method="post">
    <input type="text" name="username" placeholder="유저네임" required="required" />
    <input type="password" name="password" placeholder="비밀번호" required="required" />
    <button>로그인</button>
</form>
```


