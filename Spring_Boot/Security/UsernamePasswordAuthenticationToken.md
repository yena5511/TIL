## UsernamePasswordAuthenticationToken

- Authentication을 implements한 AbstractAuthenticationToken의 하위 클래스
- User의 ID가 Principal역할을 하고, Password가 Credential의 역할을 한다.
- UsernamePasswordAuthenticationToken의 첫 번째 생성자는 인증 전의 객체를 생성하고, 두번째 생성자는 인증이 완려된 객체를 생성한다.

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

#### UsernamePasswordAuthenticationFilter

- 사용자의 로그인 request를 제일 먼저 만나는 컴포넌트는 바로 Spring Security Filter Chain의 UsernamePasswordAuthenticationFilter 이다
- 일반적으로 로그인 폼에서 제출되는 Username과 Password를 통한 인증을 처리하는 Filter이다.
- UsernamePasswordAuthenticationFilter 는 클라이언트로부터 전달 받은 Username과 Password를 Spring Security가 인증 프로세스에서 이용할 수 있도록 UsernamePasswordAuthenticationToken 을 생성한다.

- AuthenticationManager를 통한 인증 실행
- 성공하면, Authentication 객체를 SecurityContext에 저장 후 AuthenticationSuccessHandler 실행
- 실패하면 AuthenticationFailureHandler 실행