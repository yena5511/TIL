## Filter

![](https://velog.velcdn.com/images%2Fseongwon97%2Fpost%2F8b687d86-af0f-4fd3-aed5-5a2c996a15b8%2Ffig-1-servlet-container.png)

- 서블릿 컨테이너의 Filter는 Dispatch Survlet으로 가기전에 먼저 적용된다.
- Filter들은 여러개가 연결되어 있어 Filter chain이라고 불린다.
- 모든 Request들은 Filter chain을 거쳐야지 Survlet에 도착하게 된다.

![](https://velog.velcdn.com/images%2Fseongwon97%2Fpost%2F900e612a-9e5a-4c08-a93f-444521f124d7%2Ffig-2-spring-big-picture.png)

- Spring Security는 DelegatingFilterProxy라는 필터를 만들어 메인 Filter Chain에 끼워 넣고, 그 아래 다시 SecurityFilterChain 그룹을 등록한다.
- 그렇게 하며 URL에 따라 적용되는 Filter Chain을 다르게 하는 방법을 사용한다.
- 어떠한 경우에는 해당 Filter를 무시하고 통과하게 할 수도 있다.
- `WebSecurityConfigurerAdapter`는 Filter chain을 구성하는 Configuration클래스로 해당 클래스의 상속을 통해 Filter Chain을 구성할 수 있다.
- `configure(HttpSecurity http)`를 override하며 filter들을 세팅한다.

``` java
@EnableWebSecurity(debug = true) // request가 올 떄마다 어떤 filter를 사용하고 있는지 출력을 해준다.
@EnableGlobalMethodSecurity(prePostEnabled = true) // 사전에 prePost로 권한체크를 하겠다는 설정!!
public class SecurityConfig extends WebSecurityConfigurerAdapter {
// WebSecurityConfigurerAdapter`는 Filter chian을 구성하는 Configuration클래스

    @Override
    protected void configure(HttpSecurity http) throws Exception {
    // HttpSecurity를 설정하는 것이 Filter들을 Setting하는 것이다.
        http.authorizeRequests((requests) ->
                requests.antMatchers("/").permitAll()
                        .anyRequest().authenticated()
        );
        http.formLogin(login->
                login.defaultSuccessUrl("/", false));
       http.httpBasic();
    }
}
```

#### Filter 종류

|Filter|설명|
|:---|:---|
|HeaderWriterFilter|Request의 Http 헤더를 검사하여 header를 추가하서나 뺴주는 역할을 한다.|
|CorsFilter|허가된 사이트아 클라이언트의 요청인지 검사하는 역항을 한다.|
|CsrfFilter|Post나 Put과 같이 리소스를 변경하는 요청의 걍우 내가 내보냈던 리소스에서 올라온 요청인지 확인한다.|
|LogoutFilter|Request가 로그아웃하겠다고 하는것인지 체크한다.|
|UsernamePasswordAuthenticationFilter|username/password로 로그인을 하려고 하는지 체크하여 승인이 되면 Authentication을 부여하고 이동 할 페이지로 이동한다.|
|ConcurrentSessionFilter|동시 접속을 허용할지 체크한다.|
|BearerTokenAuthenticationFilter | Authorization 해더에 Bearer 토큰을 인증해주는 역할을 한다.|
|BasicAuthenticationFilter | Authorization 해더에 Basic 토큰을 인증해주는 역할을 한다.|
|RequestCacheAwareFilter| request한 내용을 다음에 필요할 수 있어서 Cache에 담아주는 역할을 한다. 다음 Request가 오면 이전의 Cache값을 줄 수 있다.|
|SecurityContextHolderAwareRequestFilter|보안 관련 Servlet 3 스펙을 지원하기 위한 필터라고 한다.|
|RememberMeAuthenticationFilter|아직 Authentication 인증이 안된 경우라면 RememberMe 쿠키를 검사해서 인증 처리해준다.|
|AnonymousAuthenticationFilter|앞선  필터를 통해 인증이 아직도 안되었으면 해당 유저는 익명 사용자라고 Authentication을 정해주는 역할을 한다. (Authentication이 Null인 것을 방지!!)|
|SessionManagementFilter|서버에서 지정한 세션정책에 맞게 사용자가 사용하고 있는지 검사하는 역항을 한다.|
|ExcpetionTranslationFilter|해당 필터 이후에 인증이나 권한 예외가 발생하면 해당 필터가 처리를 해준다.|
|FilterSecurityInterceptor|사용자가 요청한 request에 들어가고 결과를 리턴해도 되는 권한(Authorization)이 있는지를 체크한다. 해당 필터에서 권한이 없다는 결과가 나온다면 위의 ExcpetionTranslationFilter필터에서 Exception을 처리해준다.|

#### Filter Chain 확인하는 방법

WebSecurityConfigurerAdapter을 상속받아 Filter Chain을 만드는 Class위에 @EnableWebSecurity(debug = true)어노테이션을 붙여주면 현재 실행되는 Security Fiter들을 확인할 수 있다.


```java

@Configuration
@EnableWebSecurity // 시큐리티 활성화 -> 기본 스프링 필터체인에 등록
public class SecurityConfig extends WebSecurityConfigurerAdapter{

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private CorsConfig corsConfig;

    private final String secret = "sosow0212"; // Use your actual secret key

    @Bean
    public BCryptPasswordEncoder encoder() {
        // DB 패스워드 암호화
        return new BCryptPasswordEncoder();
    }

    @Bean
    public JwtFilter jwtFilter(){
        return new JwtFilter(secret);
    }

    @Bean
    public HttpFirewall allowUrlEncodedSlashHttpFirewall() {
        DefaultHttpFirewall firewall = new DefaultHttpFirewall();
        firewall.setAllowUrlEncodedSlash(true);
        return firewall;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .addFilter(corsConfig.corsFilter())
                .addFilterBefore(jwtFilter(), UsernamePasswordAuthenticationFilter.class)
                .csrf().disable()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .formLogin().disable()
                .httpBasic().disable()
                .authorizeRequests()
                .antMatchers("/api/v1/user/**")
                .access("hasRole('ROLE_USER') or hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
                .antMatchers("/api/v1/manager/**")
                .access("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
                .antMatchers("/api/v1/admin/**")
                .access("hasRole('ROLE_ADMIN')")
                .anyRequest().permitAll();
    }
}
```

#### Fulter Chain이 적용되는 URL 설정하는 방법

- 해당 filter를 동작시킬 URL을 설정하여면 `http.antMatcher`를 통해  
설정해야한다.
- 모든 request에 대해 동작하려면 다음과 같이 `http.antMatcher("/**")`로 하며 /api~~에 대해서 적용을 하고 싶다면 `http.antMatcher("/api/**")` 와 같이 해줘야한다.
- 여러 종류의 URL에 대해 여러 Filter를 만들고 싶다면 SecurityConfig 클래스를 여러개 만들어줘야하며 여러개의 sequrityConfig은 순서가 중요하여 class위에 `@Order `annotation을 붙여줘야한다.

```java
@Order(1) // order가 낮은 것부터 먼저 설정을 한다.
@EnableWebSecurity(debug = true)
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
    
     	http.antMatcher("/**"); // 해당 filter가 동작할킬 URL을 설정하려면 antMatcher를 통해 설정해야한다.
       
        http.authorizeRequests((requests) ->
                requests.antMatchers("/").permitAll()
                        .anyRequest().authenticated()
        );
        http.formLogin(login->
                login.defaultSuccessUrl("/", false));
       http.httpBasic();
    }
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fryt33%2FbtrUWSzQ8B7%2FUyJhUb6DqYeYkP1MikzlBk%2Fimg.png)

우선 Filter은 DispatchServlet 앞에서 동작하는 큰 문이고, Interceptor은 DispatchServlet과 Controller 사이에서 동작하는 작은 문이다.

#### 필터
- Web Application Context의 기능
- 인코딩, Cors, Log, 인증, 권한 들을 구현

#### 인터셉터
- 스프링에서 SPring Context의 기능 Bean같은 느낌
- 스프링 컨테이너로 다른 빈을 주입하여 사용하기 용이하다.
- 다른 빈을 활용하여 인증, 권한 등을 구현 할 수 있다.

## OncePerRequestFilter

필터의 동작 방식을 생각해보면 하나의 서블릿을 실행하는 동안 다른 서블릿에 대한 다른 요청이 들어올 수 있다. 
이때 다른 서블릿에도 같은 필터가 존재한다면 그 필터가 또 실행된다. 
그럼 필터가 중복으로 실행 되는데 이는 리소스 낭비를 하는 상황이 발생한다.
OncePerRequestFilter은이를 방지해준다.

해당 요청에 대해서 이 필터는 한 번만 실행 되는것을 지정한다

![](https://blog.kakaocdn.net/dn/bIGzIR/btrUV3hmBRh/IyDKNe12kOFpsN4kRKGnb1/img.png)

공식 문서에서 Single execution per request dispatch, on any servlet container 부분이 눈에 들어온다.

이는 우리가 OncePerRequestFilter을 통해서 동일한 request안에서는 필터링을 한번만 할 수 있게 도와준다.

위의 기능으로 인증, 인가를 거치고 url로 포워딩하면, 포워딩 요청의 인증 인가필터를 다시 거치지 않고 다음 로직을 바로 실행하게 해준다.