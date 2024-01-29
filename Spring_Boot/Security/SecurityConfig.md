## Security Config

#### HTTPSecurity 설정

스프링 시큐리티의 각종 설정은 HttpSecurity로 대부분 하게 된다.
- 리소스 접근 권한 설정
- 인증 전체 흐름에 필요한 Login, Logout 페이지 인증완료 후 페이지 인증 실패 시 이동페이지 등등
- 인증 로직을 커스텀하기위한 커스텀 필터 설정
- 기타 csrf, 강제 https호출 등등 거의 모든 스프링시큐리티의 설정

```java
  @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
              
                .csrf().disable()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .formLogin().disable()
                .httpBasic().disable();
    }
```

- csrf().disable()
: 위변조 방지 미사용 >> 토큰 방식 사용
- cors().configurationSource(corsFilter())
: corsFilter 설정
- sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) /
: session 미사용 >> 토큰 방식 사용
- formLogin().disable()
: 로그인 폼 미사용 >>
UsernamePasswordAuthenticationFilter 미동작 딸로 필터 등록 필요함
formLogin()을 사용하게 되면 서버로 로그인 요청시, username과 password로 인증하는 로직(UserDetailsService)을 수행하고, 세션에 저장한다. >> 
토큰 발급할 것이므로 비활성
- httpBasic().disable()
: 	httpBasic방식 미사용 >> Bearer방식 사용

#### httpBasic().disable()

세션방식에서 로그인시, 웹브라우저 쿠키에 세션 ID저장, 요청시 헤더에 쿠키(세션 아이다)를 들고 감

그래서 동일 도메인에서만 허용하도록 httpOnly를 사용 -> ajax나 axios로 요청시, 쿠키를 담아서 보내면 거부하는 기능

httpOnly : http요청이 아니면 쿠키를 건드릴 수 없게함, 아마 SSR으로 추측됨..
http요청이 아닌것 : 스크립트 파일에서.. fetch() . ajax, axios, 이거는 Rest Api 서버로 추측

래서 요청시 인증하기 위해 나온게(as-is: JSESSION? , to-be: Authentication)
인증시 headers.Authorization에 인증정보를 담고가는 2가지 방식이 있다.

1. header에 Authorization에 ID와 패스워드를 담는 방식 >> httpBasic 방식
- 암호화 안됨 중간에 탈퇴 위혐 > https 서버를 사용해야함

2. 토큰을 담는 방식 (JWT)>> Bearer 방식
- 노출이 되면 안되지만 id, password 보다 안전
- 서버쪽에서 계속 새로 만들어줌, 만료 시간 이후 재생성

#### 리소스의 권한 설정
특정 리소스의 접근 허용 또는 특정 권한을 가진 사용자만 접근을 가능하게 할 수 있다.

```java
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/login**", "/web-resources/**", "/actuator/**").permitAll()
                .antMatchers("/admin/**").hasAnyRole("ADMIN")
                .antMatchers("/order/**").hasAnyRole("USER")
                .anyRequest().authenticated();
    }
```

**antMatchers**
```java
 antMatchers("/login**", "/web-resources/**", "/actuator/**")
```
  - 특정 리소스에 대해서 권한을 설정한다.

**permitAll**
```java
antMatchers("/login**", "/web-resources/**", "/actuator/**").permitAll() 
```
  - antMatchers 설정한 리소스의 접근을 인증절차 없이 허용한다는 의미이다.

**hasAnyRole**
```java
antMatchers("/admin/**").hasAnyRole("ADMIN")
```
  - 리소스 admin으로 시작하는 모든 URL은 이증후 ADMIN 권한을 가진 사용자만 접근을 허용 한다는 의미이다.

**anyRequest**
```java
anyRequest().authenticated()
```
  - 모든 리소스를 의미하며 접근허용 리소스 및 인증후 특정 권한을 가진 사용자만 접근한 리소스를 설정하고 그 외 나머지 리소스들은 무조건 인증을 완료해야 접근이 가능하다는 의미이다.

#### 로그인처리 설정

가장 일반적인 로그인 방식인 로그인 FORM 페이지를 이용하여 로그인하는 방식을 사용하려고 할 때 여러가지 설정을 할 수 있다.
중요한 사실은 커스텀 필터를 적용 할 때와 여러가지 설정이 중복되거나 서로 상관없는 설정이 겹치게 되어 헷갈리는 부분이 있다.

```java
  http.formLogin()
                .loginPage("/login-page")
                .loginProcessingUrl("/login-process")
                .defaultSuccessUrl("/main")
                .successHandler(new CustomAuthenticationSuccessHandler("/main"))
                .failureUrl("login-fail")
                .failureHandler(new CustomAuthenticationFailureHandler("/login-fail"))
                
    }
```

**formLogin**

로그인 페이지와 기타 로그인 처리 및 성공 실패 처리를 사용하겠다는 의미이다.

```java
http.formLogin();  
```
- 일반적인 로그인 방식 즉 로그인 폼 페이지와 로그인 처리 성공 실패 등을 사용하겠다는 의미이다. 
http.formLogin() 를 호출하지 않으면 완전히 로그인처리 커스텀필터를 만들고 설정하지 않는 이상 로그인 페이지 및 기타처리를 할 수 가 없다. 
커스텀 필터를 만들면 사실상 필요 없는 경우도 있다.

**loginPage**

사용자가 따로 만든 로그인 페이지를 사용하려고 할 때 설정한다.
```java
loginPage("/login-page") 
```
- 따로 설정하지 않으면 디폴트 URL이 “/login”이기 때문에 “/login”로 호출하면 스프링이 제공하는 기본 로그인페이지가 호출된다. 

**loginProcessingUrl**

로그인 즉 인증 처리를 하는 URL을 설정한다. 
“/login-process” 가 호출되면 인증처리를 수행하는 필터가 호출된다.

```java
loginProcessingUrl("/login-process") 
```
- 로그인 FORM에서 아이디와 비번을 입력하고 확인을 클릭하면 “/login-process” 를 호출 하게 되었들 때 인증처리하는 필터가 호출되어 아이디 비번을 받아와 인증처리가 수행되게 된다.
즉 UsernamePasswordAuthenticationFilter가 실행 되게 되는 것이다.

**defaultSuccessUrl**
정상적으로 인증성공 했을 경우 이동하는 페이지를 설정한다.

```java
defaultSuccessUrl("/main")
```
- 설정하지 않는 경우 디폴트 값은 "/"이다.

**successHandler**

정상적인 인증 성공 후 별도의 처리가 필요한 경우 커스텀 핸들러를 생성하여 등록할 수 있다.

```java
successHandler(new CustomAuthenticationSuccessHandler("/main"))
```
- 커스텀 핸들러를 생성하여 등록하면 인증성공 후 사용자가 추가한 로직을 수행하고 성공 페이지로 이동한다.

**failureUrl**

인증이 실패 했을 경우 이동하는 페이지를 설정한다.
```java
failureUrl("/login-fail")
```

**failureHandler**

인증 실패 후 별도의 처리가 필요한 경우  커스텀 핸들러를 생성하여 등록 할 수 있다.
```java
successHandler(new CustomAuthenticationFailureHandler("/login-fail"))
```
- 커스텀 핸들러를 생성하여 등록하면 인증실패 후 사용자가 추가한 로직을 수행하고 실패 페이지로 이동한다.

#### 커스텀 필터 등록

스프링시큐리티는 각각역할에 맞는 필터들이 체인형태로 구성되서 순서에 맞게 실행되는 구조로 동작한다. 
사용자는 특정 기능의 필터를 생성하여 등록할 수 있다. 
인증을 처리하는 기본필터 UsernamePasswordAuthenticationFilter 대신 별도의 인증 로직을 가진 필터를 생성하고 사용하고 싶을 때 아래와 같이 필터를 등록하고 사용한다.

```java
@EnableWebSecurity
public class BrmsWebSecurityConfiguration extends WebSecurityConfigurerAdapter {
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.addFilterBefore(new CustomAuthenticationProcessingFilter("/login-process"), 
                UsernamePasswordAuthenticationFilter.class);         
    }
} 
```

**addFilterBefore**

저장된 필터 앞에 커스텀 필터를 추가 (UsernamePasswordAuthenticationFilter 보다 먼저 실행된다.)

**addFilterAfter**

지정된 필터 뒤에 커스텀 필터를 추가 (UsernamePasswordAuthenticationFilter 다음에 실행된다.)

**addFilterAt**

지정된 필터의 순서에 커스텀 필터가 추가된다.

어떤 메소드로 커스텀 필터를 추가하더라도 기존 필터가 오버라이드 되는 메소드는 없다. 
다만 커스텀 필터가 실행 되고 인증이 완료 되었기 때문에 UsernamePasswordAuthenticationFilter 수행되면서 인증완료된 상태이면 인증 로직이 수행되지 않고 자연스럽게 통과 하기 때문에 마치 오버라이드된 것 처럼 동작하는 것으로 착각 할 수 있다.