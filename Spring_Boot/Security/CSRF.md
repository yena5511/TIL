## CSRF

Coross site Request forgery로 사이즈간 위조 요청인데, 즉 정상적인 사용자가 의도치 않은 위조요청을 보낸 것을 의미한다.
예를 들어 A라는 도메인에서, 인증된 사용자 H가 위조된 requset를 포한한 link, email을 사용하였을 경유(클릭, 또는 사이트 방문만으로도), A도메인에서는 이 사용자가 일반 유저인지 악용된 공격인지 구분 할 수 없다.


CSRF protection은 spring security에서 default로 설정된다.
protection을 통해 GET요청을 제외한 상태를 변화시킬 수 있는 POST, PUT, DELETE 요청으로부터 보호한다.

csrf protection을 적용하였을 때, html에서 다음과 같은 csrf 토큰이 포함되어야 요청을 받아들이게 됨으로써, 위조 요청을 방지하게 된다.

```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>
```

## Rest api에서의 CSRF

 CSRF를 왜 disable 하였을까? 
 spring security documentation에 non-browser clients 만을 위한 서비스라면 csrf를 disable 하여도 좋다고 한다.

 이유는 rest api를 이용한 서버라면, session 기반 인증과는 다르게 stateless하기 때문에 서버에 인증정보를 보관하지 않는다. 
 rest api에서 client는 권한이 필요한 요청을 하기 위해서는 요청에 필요한 인증 정보를(OAuth2, jwt토큰 등)을 포함시켜야 한다. 
 따라서 서버에 인증정보를 저장하지 않기 때문에 굳이 불필요한 csrf 코드들을 작성할 필요가 없다.

## 예시

```java

@Configuration
@EnableWebSecurity // 시큐리티 활성화 -> 기본 스프링 필터체인에 등록
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private CorsConfig corsConfig;

    @Bean
    public BCryptPasswordEncoder encoder(){
        // DB 패스워드 암호화
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception{
        http
                .addFilter(corsConfig.corsFilter())
                .csrf().disable()
```