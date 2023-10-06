## Spring Security

- Spring Security는 Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크이다
- Spring Security는 '인증'과 '권한'애 대한 부분을 Filter 흐름에 따라 처리하고 있다
- 보안과 과현해서 체계적으로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안 관련 로직을 작성하기 않아도 된다는 장점이 있다

#### 보안 용어 정리

- 접근 주체(Principal): 보호된 대상에 접근하는 유저
- 인증(Authenticate): 현재 유저가 누구안지 확인(ex.로그인)
    - 애플리케이션의 작업을 수행할 수 있는 주체임을 증명
- 인가(Authorize): 현재 유저가 어떤 서비스, 페이지에 접근할 수 있는 권한이 있는지 검사
- 권한: 인증된 주체가 애플리케이션의 동작을 수핼할 수 있도록 허락되었는지를 결정
    - 권한이 승인이 필요한 부분으로 접근하려면 인증과정을 통해 주체가 증면되어야만 한다
    - 권한 부여 영역
        - 웹 요청 권한
        - 메소드 호출 및 도메인 인스턴스에 대한 접근 권한

#### 특징

- 인증 및 권한 부여에 대한 포괄적이고 확장 가능한 지원
- 세션 고정, 클릭재킹,크로스 사이트 요청 위조 등과 같은 공격으로부터 보호
- 서블릿 API 통합
- Spring Wep MVC와의 선택적 통합
- Filter를 기반으로 동작
    - SpringMVC와 분리되어 관리하고 동작할 수 있다
- bean으로 설정할 수 있다
    - Spring Security 3.2부터 XML 설정을 이용하지 않아도 된다

#### Spring security는 세션/쿠키방식으로 인증

1. 유저가 로그인을 시도(http request)
2. AuthenticationFilter에서부터 위와같이 user DB까지 타고 들어감
3. DB에 있는 유저라면 UserDetails로 꺼내는 유저의 session 생성
4. Spring security의 인메모리 세션 저장소링 SecurityContextHolder에 저장
5. 유저에게 session ID와 함께 응답을 내려줌
6. 이후 요청에서는 요청쿠키에서 JSESSIONID를 까봐서 검증 후 유효하면 Authentication를 쥐어준다


#### 인증과 인가

- 인증(Authentication): 해당 사용자가 본인이 맞는지를 확인하는 절차
- 인가(Authorization): 인증된 사용자가 요청한 자원에 접근 가능한지를 결정하는 절차


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoMnop%2FbtqEJh4jxCX%2FPhClhzrEpVOCRK7wYM5R4k%2Fimg.png)

- Spring Security는 기본적으로 인증 절차를 거친 후에 인가 절차를 진행하게 되며, 인가 과정에서 해당 리소스에 대한 접근 권한이 있는지 확인을 하게 된다
- Spring Security에서는 이러한 인증과 인가를 위해 Principal을 아이디로, Credential을 비밀번호로 사용하는 Credential 기반의 인증 방식을 사용
    - Principal(접근 주체): 보호받는 Resource에 접근하는 대상
    - Credential(비밀번호): Resource에 접근하는 대상의 비밀번호


## Spring Security 모듈

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAbFLx%2FbtqEJC1tYaJ%2FBDq9cRTqiDarlBa3Z05FoK%2Fimg.png)

#### [SecurityContextHolder]

- 보안 주체의 세부 정보를 포함하여 응용프로그램의 현재 보안 컨텍스트에 대한 세부 정보가 저장 된다
-  SecurityContextHolder는 기본적으로 SecurityContextHolder.MODE_INHERITABLETHREADLOCAL 방법과SecurityContextHolder.MODE_THREADLOCAL 방법을 제공한다

[SecurityContext]
- Authentication(인증)을 보관하는 역할을 하며, SecurityContext를 통해 Authentcation(인증)객체를 꺼내올 수 있다

[Authentication]

- 모든 접근 주체(=유저) 는 Authentication 를 생성한다
- 현재 접근하는 주체의 정보와 권한을 담는 인터페이스이다
- Authentication 객체는 Security Context에 저장되며, SecurityContextHolder를 통해 SecutityContext애 접근하고, SrcurityContext를 통해 Authentication에 접근할 수 있다


[UsernamePasswordAuthenticationToken]
- UsernamePasswordAuthenticationToken은 Authentication을 implements한 AbstractAuthentication의 하위 클래스로, User의 ID가 Principal 역할을 하고, Password가 Credental의 역할을 한다
- UsernamePasswordAuthenticationToken의 첫 생성자는 인증 전의 객체를 생성하고, 두 번째 생성자는 인증이 완려된 객체를 생성한다

```java
public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
    // 주로 사용자의 ID에 해당함
    private final Object principal;
    // 주로 사용자의 PW에 해당함
    private Object credentials;
    
    // 인증 완료 전의 객체 생성
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
		super(null);
		this.principal = principal;
		this.credentials = credentials;
		setAuthenticated(false);
	}
    
    // 인증 완료 후의 객체 생성
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
			Collection<? extends GrantedAuthority> authorities) {
		super(authorities);
		this.principal = principal;
		this.credentials = credentials;
		super.setAuthenticated(true); // must use super, as we override
	}
}


public abstract class AbstractAuthenticationToken implements Authentication, CredentialsContainer {
}
```

[AuthenticationProvider]

- AuthenticationProvider에서는 실제 인증엘 대한 부분을 처리하는데, 인증 전의 Authentication객체를 받아서 인증이 완료된 객체를 반환하는 역할을 한다
- 아래와 같이 AuthenticationProvider 인테페이스를 구현해서 Custom한 AuthenticationProvider을 작성해서 AuthenticationManager에 등록하면 된다
```java
public interface AuthenticationProvider {

	// 인증 전의 Authenticaion 객체를 받아서 인증된 Authentication 객체를 반환
    Authentication authenticate(Authentication var1) throws AuthenticationException;

    boolean supports(Class<?> var1);
    
}
```

[Authentication Manager]

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99CD2C3A5B6B2A1903)
- 인증에 대한 부분은 Spring Security의 AuthenticatonManager를 통해서 처리하게 되는데, 실질적으로는 AuthenticationManager에 등록된 AuthenticationProvider에 의해 처리된다
- 인증이 성공하면 2번째 생성자를 이용해 인증이 성공한 (isAuthenticated = true) 객체를 생성하여 Security Context에 저장한다
- 그리고 인증 상태를 유지하기 위해 세션에 보관하며, 인증이 실패한 경우에는 AuthrnticationException을 발생시킨다
```java
public interface AuthenticationManager {
	Authentication authenticate(Authentication authentication) 
		throws AuthenticationException;
}
```

- AuthenticationManager를 implements한 ProvoderManager는 실제 인증 과정에 대한 로직을 가지고 있는 AuthenticaionProvider를 List로 가지고 있으며, ProividerManager는 for문을 통해 모든 provider를 조화하면서 authenticate 처리한다

```java
public class ProviderManager implements AuthenticationManager, MessageSourceAware,
InitializingBean {
    public List<AuthenticationProvider> getProviders() {
		return providers;
	}
    public Authentication authenticate(Authentication authentication)
			throws AuthenticationException {
		Class<? extends Authentication> toTest = authentication.getClass();
		AuthenticationException lastException = null;
		Authentication result = null;
		boolean debug = logger.isDebugEnabled();
        //for문으로 모든 provider를 순회하여 처리하고 result가 나올 때까지 반복한다.
		for (AuthenticationProvider provider : getProviders()) {
            ....
			try {
				result = provider.authenticate(authentication);

				if (result != null) {
					copyDetails(authentication, result);
					break;
				}
			}
			catch (AccountStatusException e) {
				prepareException(e, authentication);
				// SEC-546: Avoid polling additional providers if auth failure is due to
				// invalid account status
				throw e;
			}
            ....
		}
		throw lastException;
	}
}
```

- 위에서 설명한 ProviderManager에 우리가 직접 구현한 CustomAuthenticationProvider를 등록하는 방법은 WepSecurityConfigurerAdapter를 상속해 만든 SecurityConfig에서 할 수 있다
- WepSecurityConfigurerAdapter의 상위 클래스에서는 AuthenticationManager를 가지고 있기 때문에 우리가 직접 만든 CustomAuthenticationProvider를 등록할 수 있다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  
    @Bean
    public AuthenticationManager getAuthenticationManager() throws Exception {
        return super.authenticationManagerBean();
    }
      
    @Bean
    public CustomAuthenticationProvider customAuthenticationProvider() throws Exception {
        return new CustomAuthenticationProvider();
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.authenticationProvider(customAuthenticationProvider());
    }
}
```

[UserDetails]

- 인증에 성공하여 생성된 UserDetails 객체는 Authentication객체를 구현한 UsernamePassworldAuthenticationToken을 생성하시 위해 사용된다
- UserDetails 인터페이스를 살펴보면 아래와 같이 정보를 반환하는 메소드를 가지고 있다
- UserDetails 인터페이스의 경우 직잡 개발한 UserVO모델에 UserDatails를 implements하여 이를 처리하거나 UserDetailsVO에 UserDetails를 implements하여 처리할 수 있다

```java
public interface UserDetails extends Serializable {

    Collection<? extends GrantedAuthority> getAuthorities();

    String getPassword();

    String getUsername();

    boolean isAccountNonExpired();

    boolean isAccountNonLocked();

    boolean isCredentialsNonExpired();

    boolean isEnabled();
    
}
```

[UserDetailService]

- UserDetailsService 인터페이스는 UserDetalis 객체를 반환하는 단 하나의 메소드를 가지고 있는데, 일반적으로 이를 구현한 클래스의 내부에 UserRepository를 주입받아 DB와 연결하여 처리한다
- UserDetails 인터페이스는 아래와 같다
```java
public interface UserDetailsService {

    UserDetails loadUserByUsername(String var1) throws UsernameNotFoundException;

}
```

[Password Encoding]
- AuthenticationMangerBuilder.userDetailsService().passwordEncoder()를 통해 패스워드 암호화에 사용될 PasswordEncoder 구현체를 지정할 수 있다
```java
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
	// TODO Auto-generated method stub
	auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
}

@Bean
public PasswordEncoder passwordEncoder(){
	return new BCryptPasswordEncoder();
}
```

[GrantedAuthority]

- GrantAuthority는 현재 사용자(principal)가 가지고 있는 권한을 의미한다
- ROLE_ADMIN나 ROLE_USER와 같이 ROLE_*의 형태로 사용하며, 보통 "roles"이라고 한다
- GrantedAuthority 객체는 UserDetailsService에 의해 불러올 수 있고 특정 자원에 대한 권한이 있는지를 검사하여 접근 허용 여부를 결정한다


## Securing a Web Application

#### 보안되지 않는 웹 애플리케이션 생성

웹 애플리테이션에 보안을 적용하려면 먼저 보안을 설정할 웹 애플리케이션이 필요하다

웹 애플리케이션에는 홈 페이지와 "Hello, World"페이지하는 두 가지 간단한 보기가 포함되어 있다
 홈 페이지는 다음 Thymeleaf 템플릿으로 정의

`home`
 ```html
 <!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org">
    <head>
        <title>Spring Security Example</title>
    </head>
    <body>
        <h1>Welcome!</h1>

        <p>Click <a th:href="@{/hello}">here</a> to see a greeting.</p>
    </body>
</html>
 ```


`hello`
```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org">
    <head>
        <title>Hello World!</title>
    </head>
    <body>
        <h1>Hello world!</h1>
    </body>
</html>
```

웹 애플리케이션은 spring MVC를 기반으로 한다
결과적으로 Spring MVC를 구성하고 이러한 템플릿을 노출하도록 뷰 컨트롤러를 설정해야 한다

`MvcConfig`
```java
package com.example.securingweb;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MvcConfig implements WebMvcConfigurer {

	public void addViewControllers(ViewControllerRegistry registry) {
		registry.addViewController("/home").setViewName("home");
		registry.addViewController("/").setViewName("home");
		registry.addViewController("/hello").setViewName("hello");
		registry.addViewController("/login").setViewName("login");
	}

}
```

```addViewControllers()```메서드(에서 동일한 이름의 메서드를 재정의함 WebMvcConfigurer)는 4개의 뷰 컨트롤러를 추가한다
```home```뷰 컨트롤러 중 두개는 이름이 (에 정의됨)인 뷰를 참조하고, 다른 하나는 ( 에 정의되어 있음 ) home.html이라는 이름의 뷰를 참조한다
네 번째 뷰 컨트롤러는 다른 뷰를 참조한다

## 스프링 보안 설정

승인되지 않은 사용자가 인사말 페이지를 보는 것을 방지한다고 가정해 보겠다
```/hello```
지금부터 방문자가 홈페이지의 링크를 클릭하면 아무헌 문제 없이 인사말을 볼 수 있습니다
방문자가 해장 페이지를 보기 전에 로그인하도록 강제하는 장벽을 추가해야 한다

애플리케이션에서 Spring Security를 구성하면 된다
SpringSecurity가 클래스 경로에 있는 경유 spring Boot는 기본 인증을 사용하여 모든 HTTP끝점을 자동으로 보호한다
그러나 보안설정을 추가로 사용자 정의할 수 있다
가장먼저 해야할 일은 클래스 경로에 Spring security를 추가하는 것 이다

Gredle을 사용하면 다음 목록과 같이 ```dependencies```클로저에 세 줄(애플리케이션용 하나, Thymeleaf 및 Spring Security통합용 하나, 테스트용 하나)를 추가해야 한다

```java
implementation 'org.springframework.boot:spring-boot-starter-security'
//  Temporary explicit version to fix Thymeleaf bug
implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6:3.1.1.RELEASE'
implementation 'org.springframework.security:spring-security-test'
```

인증된 사용자만 비밀 인사말을 볼 수 있도록 보장한다

`WebSecurityConfig`
```java
package com.example.securingweb;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class WebSecurityConfig {

	@Bean
	public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
		http
			.authorizeHttpRequests((requests) -> requests
				.requestMatchers("/", "/home").permitAll()
				.anyRequest().authenticated()
			)
			.formLogin((form) -> form
				.loginPage("/login")
				.permitAll()
			)
			.logout((logout) -> logout.permitAll());

		return http.build();
	}

	@Bean
	public UserDetailsService userDetailsService() {
		UserDetails user =
			 User.withDefaultPasswordEncoder()
				.username("user")
				.password("password")
				.roles("USER")
				.build();

		return new InMemoryUserDetailsManager(user);
	}
}
```

이 클래스에는 Spring Security의 웹 보안 지원을 활성화하고 spring MVC통합을 제공하기 위해 ```WebSecurityConfig```주석이 추가 된다

``` @EnableWebSecurity```또 한 웹 구성에 대한 며착지 세부 사항을 위해 두 개의 Bean을 노출한다

Bean`SecurityFilterChain`은 보호해야 할 URL경로와 보호하지 말아야할 URL 경로를 정의 한다
특히 `/`및 `/home`경로는 인증이 필요하지 않도록 구성된다
다른 모든 결로는 인증되어야 한다

사용자가 성공적으로 로그인하면 인증이 필요한 이전에 요청한 페이지로 리디렉션된다
`/login`로 지정되는 사용자 정의 페이지가 있으면 `loginPage()`모든 사람이 볼 수 있어야 한다

Bean `UserDetailsService`은 단일 사용자로 메모리 내 사용자 저장소를 설정한다
해당 사용자에게는 사용자 이름 user, 암호 password및 역할이 부여된다

이제 로그인 페이지를 생성해야 한다
뷰 컨트롤러가 이미 있으므로 `login`다은 목록과 같이 로그인 뷰 자체만 생성하면 된다

```java
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org">
    <head>
        <title>Spring Security Example </title>
    </head>
    <body>
        <div th:if="${param.error}">
            Invalid username and password.
        </div>
        <div th:if="${param.logout}">
            You have been logged out.
        </div>
        <form th:action="@{/login}" method="post">
            <div><label> User Name : <input type="text" name="username"/> </label></div>
            <div><label> Password: <input type="password" name="password"/> </label></div>
            <div><input type="submit" value="Sign In"/></div>
        </form>
    </body>
</html>
```

이 Thymeleaf 템플렛은 사용자 이름과 비밀번호를 캡처하여 게시하는 양식을 제공한다
`/login`구성된 대로 springsecurity는 해당요청을 가로채고 사용자를 인증하는 필터를 제공한다
사용자가 인증에 실패하면 페이지가 리디렉션되고 `/login?error`페이지로 해당 오류 메시지가 표시 된다
로그아웃에 성공하면 애플리케이션으로 전송되고 `/login?logout`페이지에 적절한 성공메시지가 표시된다

마지막으로 방문자에게 현재 사용자 이름을 표시하고 로그아웃 할 수 있는 방범을 제공해야 한다
이렇게 하려면 다음 목록과 같이 `hello.html`현재 사용자에게 인사하고 양식을 포함하도록 업데이트를 해야 한다

```java
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org"
      xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity6">
    <head>
        <title>Hello World!</title>
    </head>
    <body>
        <h1 th:inline="text">Hello <span th:remove="tag" sec:authentication="name">thymeleaf</span>!</h1>
        <form th:action="@{/logout}" method="post">
            <input type="submit" value="Sign Out"/>
        </form>
    </body>
</html>
```

Thymeleaf와 spring Security의 통합을 사용하여 사용자 이름을 표시한다
"로그아웃"양식은 POST에 제출한다
성공적으로 로그아웃되면 사용자를 리디렉션한다

#### 애플리케이션 실행

Spring초기화는 당신을 위해 애플리케이션 클래스를 생성한다
이 경우 클래스를 수정할 필요가 없다

```java
package com.example.securingweb;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SecuringWebApplication {

	public static void main(String[] args) throws Throwable {
		SpringApplication.run(SecuringWebApplication.class, args);
	}

}
```