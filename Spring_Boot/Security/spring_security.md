## Spring Security

- Spring Security는 Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크이다
- Spring Security는 '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리하고 있다
- 보안과 관련해서 체계적으로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안 관련 로직을 작성하기 않아도 된다는 장점이 있다

#### 보안 용어 정리

- 접근 주체(Principal): 보호된 대상에 접근하는 유저
- 인증(Authenticate): 현재 유저가 누구인지 확인(ex.로그인)
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
- UsernamePasswordAuthenticationToken의 첫 생성자는 인증 전의 객체를 생성하고, 두 번째 생성자는 인증이 완료된 객체를 생성한다

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

- AuthenticationProvider에서는 실제 인증에 대한 부분을 처리하는데, 인증 전의 Authentication객체를 받아서 인증이 완료된 객체를 반환하는 역할을 한다
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

## 스프링 보안 아키텍퍼

#### 인증 및 액세스 제어

애플리케이션 보안은 인증(당신은 누구입니까?)과 승인(무엇을 할 구 있습니까?)이라는 두 가지 독립적인 문제로 요약된다
가끔 사람들이 인증 대신에 접근제어라고 말하는데 이는 혼란 스러울 수 있지만, 
인증은 다른 곳에서 과부하가 걸리기 때문에 그렇게 생삿하는 것이 도움이 될 수 있다
Spring Security에는 인증과 인증을 분리하도록 설계된 아키텍쳐가 있으며 두가지 모두에 대한 전략과 확장 지점이 있다

#### 인증

인증을 위한 기본 전략 인터페이스는 ```AuthenticationManager```다음과 같이 한 가지 방법만 있다

```java
public interface AuthenticationManager {

  Authentication authenticate(Authentication authentication)
    throws AuthenticationException;
}
```

`AuthenticationManager`해당 메서드에서는 다음 세 가지 작업 중 하나를 수행할 수 있다 `authenticate()`
- 입력이 유효한 주체를 나타내는지 확인할 수 있으면 `Authentication`(일반적으러) `authenticated=true`를 반환한다
- `AuthenticationException`입력이 유효하지 않은 주체를 나타낸다고 판단되면 발생시킨가
- `null`결정할 수 없으면 반품한다

`AuthenticationException`런타임 예외이다
일반적으로 애플리케이션의 스타일이나 목적에 따라 일반적인 방식으로 애플리케이션에서 처리 된다
즉, 사용자 코드는 일반적으로 이를 포착하고 처리할 것으로 예상되지 않는다
예를 들어 웹UI는 인증 실패를 알리는 페이지를 렌더링할 수 있으면 백엔드 HTTP서비스는 `WWW-Authenticate`컨텍스트에 따라 헤더 유무에 관계없이 401응답을 보낼 수 있다

가장 일반적으로 사용되는 구현은 인스턴스 체인에 위힘하는 것`AuthenticationManager`이다
An은 an과 약간 비슷하지만 호출자가 주어진 유형을 지원하는지 여부를 쿼리할 수 있는 추가 메서드가 있다
`ProviderManager`
`AuthenticationProvider`
`AuthenticationManager`
`Authentication`

```java
public interface AuthenticationProvider {

	Authentication authenticate(Authentication authentication)
			throws AuthenticationException;

	boolean supports(Class<?> authentication);
}
```

`Class<?>`메서드의 인수는 실제로 `supports()`이다
`Class<? extends Authentication>`(메서드에 전달된 항목을 지원하는지 여부만 묻는다 `authenticatioj()`)
A는 `ProviderManager`체인에 위임하여 동일한 애플리케이션에서 여러가지 인증 메커니즘을 지원할 수 있다
`AuthenticationProviders`, `ProviderManager`가 특정 인스턴스 유형을 인식하지 못하는 경우 `Authentication`건너 뛴다

A`roviderManager`에는 모든 공급자가 반환하는 경우 참조할 수 있는 선택적 부모가 있다 `null`
상위 항목을 사용할 수 없는 경우 `null`, `Authentication`결과입니다

경우에 따라 애플리케이션에는 보호된 리소스(예: 경로 패턴과 일치하는 모든 웹리소스 `/api/**`)의 논리적그룹이 있고 각 그룹에는 자체 전용 `AuthenticationManager`
종종 이들 각각은 `ProviderManager`이고 상위를 공유한다
그러면 상위는 일종의 "글로벌"리소스가 되어 모든 공급자에 대한 대체역할을 한다


#### `AuthenticationManager`사용한 계층 구조`ProviderManager`

![](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/authentication.png)

Spring Security는 애플리케이션에 공통 인증 관리자 기능을 빠르게 설정할 수 있도록 몇가지 구성 도우미를 제공한다
가장 일반적으로 사용되는 도우미는 `AuthenticationManagerBuilder`메모리, JDBC또는 LDAP 사용자 세부 정보를 설정하거나 사용자 정의 `UserDetailsService`
다음 예에서는 전역 (상위)을 구성하는 애플리케이션으 보여준다 

```java
@Configuration
public class ApplicationSecurity extends WebSecurityConfigurerAdapter {

   ... // web stuff here

  @Autowired
  public void initialize(AuthenticationManagerBuilder builder, DataSource dataSource) {
    builder.jdbcAuthentication().dataSource(dataSource).withUser("dave")
      .password("secret").roles("USER");
  }

}
```

이 예는 웹 애플리케이션과 관련이 있지만 사용법이 `AuthenticationManagerBuilder`더 광법위하게 적용된다
a의 매서드에 포함되어`AuthenticationManagerBuilder`있으므로 전역(부모)을 빌드하게 된다
이와 대조적인 다음 예시 `@Autowired`
`@Bean`
`AuthenticationManager`

```java
@Configuration
public class ApplicationSecurity extends WebSecurityConfigurerAdapter {

  @Autowired
  DataSource dataSource;

   ... // web stuff here

  @Override
  public void configure(AuthenticationManagerBuilder builder) {
    builder.jdbcAuthentication().dataSource(dataSource).withUser("dave")
      .password("secret").roles("USER");
  }

}
```
`@override`구성에서 메서드를 사용했다면 전역 메서드의 하위인 `AuthenticationManagerBuilder` "로컬"을 빌드하는 데에만 사용된다
`AuthenticationManager`Spring Boot 애플리케이션에서는 `@Autowried`전역 빈을 다른 빈으로 만들 수 있지만 명시적으로 직접 노출하지 않는 한 로컬 빈으로 그렇게 할 수 는 없다

`AuthenticationManager`SpringBoot는 사용자가 유형의 빈을 제공하여 선점하지 않는 한 기본 전역(사용자가 한 명만 있음)을 제공한다
기본값은 사용자 정의 전역이 적극적으로 필요하지 않는 한 크게 걱정할 필요가 없으 정도로 그 자체로 충분히 안전하다
`AuthenticationManager`를 구축하는 구성을 수행하는 경우 
보호하는 리소스에 대한 로컬로 수행할 수 있으며 전역 기본값에 대해 걱정할 필요가 없다

승인 또는 액세스 제어

인증을 성공하면 인증으로 넘어갈 수 있는데 여기서 핵심 전략은 `AccessDecisionManager`이다
프레임워크에서 제공하는 새 가지 구혀닝 있으며 세 가지 모두 인스턴스 체인에 위입한다
`AccessDecisionVoter`
`ProviderManager` `AuthenticationProviders`

An(주체를 나타냄)은 다음으로 장식된 secure를 `AccessDecisionVoter` 고려한다

```java
boolean supports(ConfigAttribute attribute);

boolean supports(Class<?> clazz);

int vote(Authentication authentication, S object,
        Collection<ConfigAttribute> attributes);
```
`Authentication`
`Object`
`ConfigAttributes`
은 밎 `Object`의 서명에서 완전히 일반적이다
이는 사용자가 액세스하려는 모든 것을 나타낸다(웹 리소스 또는 Java클래스의 메소드가 가장 일반적인 두 가지 경우이다)
이는 또한 보안에 액세스하는 데 필요한 권한 수준을 결정하는 일부 메타데이터로 보안 장식을 나타내는 매우 일반적인 것이다
인터페이스 에는 하나의 메서드(아주 일반적이며 a를 반환함 )만 있으므로 이러한 문자열은 리소스 소유자의 의도를 어떤 식으로든 인코딩하여 리소스에 액세스할 수 있는 사람에 대한 규칙을 표현한다
일반적인 이름은 사용자 역할의 이름(예: 또는 )이며 특수 형식

대부분의 사람들은 기본값을 사용한다 `AccessDecisionManager` 
즉, `AffirmativeBased` 투표자가 긍정적으로 응답하면 액세스가 허용된다
모든 사용자 정의는 새로운 것을 추가하거나 기존의 작동 방식을 수정하여 유권자에서 발생하는 경향이 있다

SpEL(Spring Expression Language) 표현식을 사용하는 것이 매우 일반적이다
`ConfigAttributes`(예: `isFullyAuthenticated() && hasRole('user')`
이는 표현식을 처리하고 이에 대한 컨텍스트를 생성할 수 있는 `AccessDecisionVoter`에서 지원된다
처리할 수 있는 표현식의 범위를 확정하려면 `SecurityExpressionRoot`및 때로는 `SecurityExpressionHandler`의 사용자 정의 구현이 필요하다

#### 웹 보안

윕 계층(UI및 HTTP 백엔드용)의 Spring Security는 Servlet을 기반으로 하므로 일반적인 `Filters`역할을 먼저 살펴보는 것이 도운이 된다
`Filters`다음 그림은 단일 HTTP요청에 대한 핸들러의 일반적인 계층을 보여준다

![](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/filters.png)

클라이언트는 애플리케이션에 요청을 보내고 컨테이너는 요청 URI의 경로를 기반으로 어떤 필터와 서블릿을 적용할지 결정한다.
기껏해야 하나의 서블릿이 단일 요청을 처리할 수 있지만 필터는 체인을 형성하므로 순서가 지정된다.
실제로 필터는 요청 자체를 처리하려는 경우 체인의 나머지 부분을 거부할 수 있다.
필터는 다운스트림 필터 및 서블릿에 사용되는 요청이나 응답을 수정할 수도 있다
필터 체인의 순서는 매우 중요하다 Spring Boot는 두가지 메커니즘을 통해 필터 체인을 관리한다
`@Bean`유형은 `@Order`
`Ordered`
`FilterRegistrationBean` 또는 구현을 `Filter`가질 수 있고 그 자체에는 API의 일부로 주문이 있다.
일부 기성필터는 서로에 대한 상대적인 순서를 알리는 데 도움이 되는 자체 상수를 정의한다
예를 들어 `SessionRepositoryFilterSpring `세션의 a `DEFAULT_ORDER`는 `Integer.MIN_VALUE + 50`체인의 초기에 있기를 좋아하지만 앞에 오는 다른 필터를 배제하지 않는다.

`Filter`Spring Security는 체인에서 단일로 설치되며 구체적인 유형은 `FilterChainProxy`곧 설명할 이유이다.
Spring Boot 애플리케이션에서 보안필터는 `@Bean`,` ApplicationContext`에 있으며 모든 요청에 적용되도록 기본적으로 설치된다.
이는 `SecurityProperties.DEFAULT_FILTER_ORDER`에 의해 정의된 위치에 설치되며, 이는 다시 고정된다.
`FilterRegistrationBean.REQUEST_WRAPPER_FILTER_MAX_ORDER`(Spring Boot 애플리케이션이 요청을 래핑하여 동작을 수정하는 경우 필터가 가질 것으로 예상하는 최대 순서)
하지만 그보다 더 많은 것이 있다
컨테이너의 관점에서 보면 Spring Security는 단일 필터이지만 그 내부에는 각각 특별한 역할을 수행하는 추가 필터가 있다
다음 이미지는 이 관계를 보여준다

![](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/security-filters.png)

#### Spring Security는 단일 물리적 `Filter`이지만 내부 필터 체인에 처리를 위임한다

`DelegatingFilterProxy`실제로 보안 필토에는 간접계층이 하나 더 있다.
일반적으로 컨테이너에 SPring일 필요는 없지만 컨테이너에 설치 된다.
일반적으로 고정 이름을 사용하여 `FilterChainProxy`항상 a인 a 에 위임한다.
내부적으로 필터 체인(또는 체인)으로 배열된 모든 보안 로직을 포함하는 것 이다
모든 필터에는 동일한 API가 있으며 (모두 서블릿 사양의 인터페이스를 구현함)나머지 체인을 거부할 수 있는 기회가 있다

동일한 최상위 수준에서 Spring Security에 의해 관리되는 여러 필터 체인이 있을 수 있으며 `FilterChainProxy`모두 컨테이너에 알려지지 않느다
Spring Security 필터는 필터체인 목록을 포함하고 일치하는 첫 번뺴 체인에 요청을 전달한다.