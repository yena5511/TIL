## Authentication Provider

![](https://user-images.githubusercontent.com/53357210/140632316-c83d4059-9c6e-4a8c-b0ee-fe031bfc8287.png)

- AuthenticationProvider에서는 실제 인증에 대한 부분을 처리하는데, 인증 전의 Authentication객체를 받아서 인증이 완료된 객체를 반환하는 역할을 한다.

- `AuthenticationProvider` 는 한 줄로 정의 하면 db에서 가져온 정보와 input 된 정보가 비교되서 체크되는 로직이 포함되어있는 인터페이스 라고 할 수 있다.
- `AuthenticationProvid`er를 상속하면 메소드의 경우 사용자의  정보가 담뎌있는 객체인 `authenticate `메소드를 오버라이드 할 수 있다.
여기서 사용자는 form 또는 로그인 로직에서 가지고온 정보라고 할 수 있다.
- `authenticate ``Authentication Manager`에서 넘겨준 인증객체(Authentication)를 가지고 메소드를 만든다.
- `Authentication`는 로그인 아이디, 로그인 비밀번호를 객체에서 가지고 올 수 있다.
- db에서 사용자 정보를 가지고 오려면 `UserDetailsService `에서 리턴해주는 값을 가지고 사용이 가능하다.
- `UserDetailsService `return 값의 경우 `UserDetails`를 리턴해준다.
- 리턴된 user 정보를 가지고 `PasswordEncoder `bean을 사용하여서 input 된 비밀번호와 매칭 시켜주면 된다.
-여기서 `PasswordEncoder `의 경우 우리가 `configuartion`에서 bean으로 등록을 시켜준 클래스가 의존성 주입되어 있다.

```java
 @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
```
- security 에서 비밀번호 인코딩에 대한 빈을 주입을 해주었다.
- 만약 해주지 않거나 커스텀 된 비밀번호 인코딩을 사용하고 싶다면

AuthenticationProvider 여기서 커스텀 해주면 된다.

```java
@RequiredArgsConstructor
@Component("authenticationProvider")
public class DomainAuthenticationProvider implements AuthenticationProvider {

    private final PasswordEncoder passwordEncoder;

    private final DomainUserDetailsService userDetailsService;

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {

        String loginId = authentication.getName();

        String password = (String) authentication.getCredentials();

        DomainUserDetails domainUserDetails = (DomainUserDetails) userDetailsService.loadUserByUsername(loginId);

        if (isNotMatches(password, domainUserDetails.getPassword())) {
            throw new BadCredentialsException(loginId);
        }

        return new UsernamePasswordAuthenticationToken(domainUserDetails, domainUserDetails.getPassword(), domainUserDetails.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return authentication.equals(UsernamePasswordAuthenticationToken.class);
    }

    private boolean isNotMatches(String password, String encodePassword) {
        return !passwordEncoder.matches(password, encodePassword);
    }

}

```






