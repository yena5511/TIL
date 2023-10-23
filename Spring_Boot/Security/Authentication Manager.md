## AuthenticationManager

- 사용자 아이디 / 비밀번호 인증을 처리하는 곳  -> 유효한 인증인지 확인
- 인자로 받은 Authentication이 유효한 인증인지 확인하고,  "Authentication" 객체를 리턴
-  유저의 요청 내에 담긴 "Authentication"를 Authentication Manager 에 넘겨주고, AuthenticationProvider에서 인증을 처리한다.
- 인증 처리하는 fiter로부터 인증처리를 지시 받는 첫번째 클래스
- ID와 Password를 Authentication인증 객체에 저장하고 이 객체를  AuthenticationManager에게 전달한다. 이 인증에 대해 관리를 하게 된다.
- AuthenticationManager 인터페이스를 실제로 구현한 것이 ProviderManager. 
- AuthenticationManager 인터페이스를 구현하면 커스텀 ProviderManager도 만들 수 있다.

```java
public interface AuthenticationManager {
	Authentication authenticate(Authentication authentication) 
		throws AuthenticationException;
}
```

AuthenticationManager를 implements한 ProviderManager는 실제 인증 과정에 대한 로직을 가지고 있는 AuthenticaionProvider를 List로 가지고 있으며, ProividerManager는 for문을 통해 모든 provider를 조회하면서 authenticate 처리를 한다.

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
- ustomAuthenticationProvider를 등록하는 방법은 WebSecurityConfigurerAdapter를 상속해 만든 SecurityConfig에서 할 수 있다. 
- . WebSecurityConfigurerAdapter의 상위 클래스에서는 AuthenticationManager를 가지고 있기 때문에 우리가 직접 만든 CustomAuthenticationProvider를 등록할 수 있다.