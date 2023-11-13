## BCryptPasswordEncoder

- 스프링 시큐리티 프레임워크에서 제공하는 클래스 중 하나로 비밀번호를 암호화하는 데 사용할 수 있는 메서드를 가진 클래스이다.

- PasswordEncoder 인터페이스를 구현한 클래스입니다.

BCryptPasswordEncoder는 위에서 언급했듯이 비밀번호를 암호화하는 데 사용할 수 있는 메서드를 제공한다.
기본적으로 웹 개발함에 있어서 사용자의 비밀번호를 데이터베이스에 저장하게 된다.
허가되지 않은 사용자가 접근하지 못하도록 기본적인 보안이 되어있을 것이다.
하지만 기본적인 보안이 되어 있더라고, 만약 그 보안이 뚫리게 된다면 비밀번호 데이터가 무방비하게 노출된다.
이런 경우를 대비해  BCryptPasswordEncoder에서 제공하는 메서드를 활용하여 비밀번호를 암호화 함으로써 비밀번호 데이터가 노출되더라도 확인하기 어렵도록 만들어 줄 수 있다.

#### 메서드 구성

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCfc2m%2FbtqTKLDvXUQ%2F8AhKi0uIKLg62MsKD5Z4Kk%2Fimg.png)

- encode(java.lang.CharSequence rawPassword)
    - 패스워드를 암호화해주는 메서드이다. 
    - 매개변수는 java.lang.CharSequence타입의 데이터를 입력해주면 된다.
    - 반환 타입은 String 타입이다.
    -똑같은 비밀번호를 해당 메서드를 통하여 인코딩하더라도 매번 다른 인코딩 된 문자열을 반환한다.

- matchers(java.lang.CharSequence rawPassword, java.lang.String encodePassword)
    - 제출된 인코딩 되지 않은 패스워드(일치 여부를 확인하고자 하는 패스워드)와 인코딩 된 패스워드의 일치여부를 확인해준다.
    - 첫 번재 매개변수는 일치 여부를 확인하고자 하는 인코딩되지 않은 패스워드를 두번쨰 매개변수는 인코딩 된 패스워드를 입력한다.
    - 반환 타입은 boolean이다.

- upgradeEncoding(java.lang.String encodePassword)
    - 더 나은 보안을 위해서 인코딩 된 암호를 다시 한번 더 인코딩해야 하는 경우에 사용
    - 매개변수는 인코딩 필요 여부를 확인하고자 하는 인코딩 된 패스워드를 입력한다.
    - 반환타입은 인코딩이 필요한 경우 true를 필요하지 않은 경우는 false를 입력한다.
    - 기본 구현에는 항상 flase를 반환한다,
    - encode() 메서드를 통해서 암호화된 패스워드들은  upgradeEncoding()을 사용했을 때 모드 기본적으로 false를 반환한다.
    
#### 사용방법

`의존성 추가`
```java
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	**implementation 'org.springframework.boot:spring-boot-starter-security'**
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-websocket'
	implementation 'org.springframework.boot:spring-boot-starter-data-redis'
```

`SecurityConfig에 BCryptPasswordEncdoer Bean으로 등록`

```java
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

`BCryptPasswordEncoder 사용`

```java
@Service
@RequiredArgsConstructor
public class TestService {
//Bean으로 등록된 BCryptPasswordEncoder 의존성 주입
	private final PasswordEncoder passwordEncoder;

	public void register(String id, String password) {
			//사용자가 입력한 암호를 encode하여 저장
			String encodedPassword = passwordEncoder.encode(password);
			Member member = new Member(id, encodedPassword);

			TestRepository.save(member);
	}	

	public boolean login(String id, String password) {
			Member member = TestRepository.find(id);
			if(passwordEncoder.matches(password, member.getPassword()) {
					return true; // 입력한 비밀번호와 저장소의 비밀번호가 일치
			} else {
					return false;
			}
	}
}
```