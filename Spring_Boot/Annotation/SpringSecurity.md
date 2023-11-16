#### @Secured

@Secured 어노테이션은 권한이 필요한 부분에 선언 할 수 있는데 Class나 Method 단위까지 지정을 할 수 있다. 
예를 들면 ROLE_ADMIN만 접근시킬 메서드가 있다면 해당 메서드위에 @Secured 어노테이션을 선언해주기만 하면 된다.

```java
ex1)접근 가능한 권한이 하나 일때

@Secured("ROLE_ADMIN")
public String encoding(String str){
 return encoder.encodePassword(str,null);
}

ex2)접근 가능한 권한이 두개 이상일때

@RequestMapping(value = "/checkAuth", method = RequestMethod.GET)
@Secured({"ROLE_ADMIN","ROLE_USER"})
public String checkAuth(Locale locale, Model model, Authentication auth) {
 UserDetailsVO vo = (UserDetailsVO) auth.getPrincipal();
 logger.info("Welcome checkAuth! Authentication is {}.", auth);
 logger.info("UserDetailsVO == {}.", vo);
 model.addAttribute("auth", auth );
 model.addAttribute("vo", vo );
 return "checkAuth";
}
```
- 적용후 해당 메서드에 권한이 없는 사용자로 접근을 하게 되면 권한이 필요하다는 403에러가 뜬다.


#### @PreAuthorize

요청이 들어와 함수를 실행하기 전에 권한을 검사한다.
Spring EL(표현식)을 사용할 수 있고, AND나 OR 같은 표현식을 사용할 수 있다.

```
// ROLE_USER와 ROLE_ADMIN 두 개의 권한을 가져야 접근 가능
@PreAuthorize("hasRole('ROLE_USER') and hasRole('ROLE_ADMIN')")
 ```

 ```java
 @PreAuthorize("hasRole('ROLE_ADMIN')")
public void deleteUser(int userId) {
    // ...
}
 ```

 ##### @PostAuthorize
 
 함수를 실행하고 클라이언트한테 응답을 하기 직전에 권한을 검사한다.

 ```java
 @PostAuthorize("returnObject.userId == authentication.principal.username")
public Order getOrder(int orderId) {
    // ...
    return order;
}
 ```

#### @AuthenticationPrincipal

세션 정보 UserDetails에 접근할 수 있는 어노테이션

`@AuthenticationPrincipal`은 `UserDetails`타입을 가지고 있음 -> `UserDetails`타입을 구현한 `PrincipalDetails`클래스를 받아 User object를 얻는다.

- userDetails(PrincipalDetails 타입).getUser()

따라서 로그인 세션 정보가 필요한 컨트롤러에서 매번 @AuthenticationPrincipal로 세션 정보를 받아서 사용한다. 

- @AuthenticationPrincipal`UserAdapter`타입
- 로그인 세션 정보를 어노테이션을 간편하게 받을 수 있다
- UserDetailsService에서 return한 객체를 파라미터로 직접 받아 사용할 수 있다
- name 뿐만 아니라 다양한 정보를 받을 수 있다
- 중복 코드를 효율적으로 줄일 수 있다


## @EnableWebSecurity

- web security 구성정보를 활성화하기 위해 사용된다.
- 스프링에서 인증 및 권한을 제공하는 보안 구성을 적용하도록 신호를 보낸다.
- 주로 `WebSecurityConfigurerAdapter` 클래스를 상속하여 사용한다.
    - 기본 보안 설정을 제공하고, 메서드 오버라이딩을 통한 보안 커스터마이징을 제공한다.   
- `@EnableWebSecurity` 어노테이션은 스프링 시큐리티를 활성화하고 웹 보안 설정을 구성하는 데 사용된다.

```java
   @Configuration
     @EnableWebSecurity
     public class SecurityConfig extends WebSecurityConfigurerAdapter {
         @Override
         protected void configure(HttpSecurity http) throws Exception {
             http
                 .authorizeRequests()
                     .antMatchers("/public/**").permitAll()
                     .anyRequest().authenticated()
                 .and()
                     .formLogin()
                 .and()
                     .logout();
         }
     }
```
- 위의 예시에서는 `/public` 경로에 대한 접근은 인증 없이 허용하고, 그 외의 모든 요청은 인증을 필요로 하다.

#### @EnableGlobalMethodSecurity

```java
@Configuration
@EnableWebSecurity // 스프링 시큐리티 필터가 스프링 필터체인에 등록이 됨
@EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true) // secured 어노테이션 활성화, preAuthorize, postAuthorize 어노테이션 활성화
public class SecurityConfig extends WebSecurityConfigurerAdapter {}
```
@EnableGlobalMethodSecurity는 Spring AOP proxy를 사용해 구현된 방법이다. 
사용하는 방법에는 총 3가지가 있는데 3가지 방법을 모두 한번에 사용할 수도 있다. 
3가지에는 securedEnabled, prePostEnabled, jsr250Enabled라는 설정이 있다. 

-  securedEnabled - @Secured
    - @Secured 어노테이션으로 인가를 처리하고 싶을 떄 true로 설정한다.
    - @Secured는 Spring Security2의 lrgacy 어노테이션이다.
    - @Secured는 SpEL(Spring Expression Languge)를 지원하지 않는다.
    - @Secured는 한 번에 여러가지 Role으로 인가 처리를 할 수 있다. 예시: (ROLE_ADMIN, ROLE_USER를 가진 유저 모두 )
    - 그러나 Legacy이기 때문에 prePostEnabled를 사용하는 것이 권장된다. 

-  prePostEnabled - @PreAuthorize

    -  @PreAuthorize를 사용하기 위해서 true로 설정한다.
    - @PreAuthorize는 SpEL을 사용해서 인가처리가 가능하다. 
    - @PreAuthorize는 스프링 시큐리티 프레임워크에 일부이다.

`SpEL 예시`
```
@PreAuthorize("hasRole('ADMIN') or #user.id == authentication.name")
```

- jsr250Enabled - @RolesAllowed
    - @RolesAllowed를 사용하기 위해서 true 설정과 스프링 시큐리티 기능이 아니기 때문에 관련된 라이브러리가 설치되어 있어야한다.
    -  @RolesAllowed는 JSR-250 Java Security Standard에 기초하고 있고 role-based 인가 기능만 있기 때문에 @PreAuthorize 어노테이션보다 제약이 많다.  

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAMOaj%2FbtrJB3eKUS6%2Fo7YpJ183YqRA21xDBrbXDK%2Fimg.png)

- Spring Security 두가지 방법
    - Web
    - Method: 메소드 별로 권한을 체크하는데 Web 기반과 다르게 필터가 아닌 컨트롤러 단에서 권한을 처리하는 기능이다.
두개의 Security 적용 방식은 하나만 선택하는게 아니라 동시에 섞어서 사용할 수 있다. 
Web Security에서 URL과 Token 유무로 인증을 하고 Method Security를 사용해서 권한에 따라 인가를 하는 방법이 있다.
Rest api에서는 Web Security에서도 권한에 따라 인가를 처리할 수 있기 때문에(antMatcher 사용) 굳이 메소드 기반을 같이 사용하지 않아도 된다. 
