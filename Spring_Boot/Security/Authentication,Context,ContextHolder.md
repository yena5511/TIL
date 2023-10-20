## Authentication(인증 객체)

사용자의 인증 정보를 저장하는 토큰 개념

2가지 용도로 사용된다
- 인증용도
- 인증 후 세션에 담기 위한 용도

1. 인증 시 id와 password를 담고 인증 검증을 위해 전달되어 사용한다
2. 인증 후 최종 인증 결과(user 객체, 권한 정보)를 담고 SecurityContext에 저장 되어 아래와 같은 코드로 전역적으로 참조가 가능하다

`Authentication authentication = SecurityContexHolder.getContext().getAuthentication()`

```java
public interface Authentication extends Principal, Serializable {
    // 현재 사용자의 권한 목록을 가져옴
    Collection<? extends GrantedAuthority> getAuthorities();
    
    // credentials(주로 비밀번호)을 가져옴
    Object getCredentials();
    
    Object getDetails();
    
    // Principal 객체를 가져옴.
    Object getPrincipal();
    
    // 인증 여부를 가져옴
    boolean isAuthenticated();
    
    // 인증 여부를 설정함
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}
```


#### Authentication 구조

1. Principal(Object 타입): 사용자 아이디 혹은 User 객체를 저장
2. Credentials: 사용자 비밀번호
3. authorities: 인증된 사용자의 권한 목록
4. details: 인증 부과 정보
5. Authenticated: 인증여부

![](https://velog.velcdn.com/images%2Fgmtmoney2357%2Fpost%2F86e9b644-b56e-4928-82e2-8a5bd40a0463%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-01%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.29.33.png)

- Authentication -> principal, credentials, authorities 필드를 가지며, 인증 전 상황과 인증 후 상황에 따라 사용되는 목적이 달라진다.
    - 인증전 - 인증을 요구하는 주체가 인증에 필요한 정보(로그인 아이디, 패스워드)를 제공하기 위해 사용
        - pricipal - 로그인 시도 이아디(String)
        - credentials - 로그인 시도 비밀번호(String)
    - 인증 후 - 인증이 완료된 사용자의 정보를 저장하는데 사용
        - principal - 인증이 완료된 사용자 객체(UserDetails의 구현체)
        - credentials - 인증 완료후 유출 가능성을 줄이기 위해 삭제
        - authorities - 인증된 사용자가 가지는 권한 목록
Authentication은 인터페이스이며, 인증하는 여러 상황에 따라 다양한 구현체로 표현된다

## SecurityContext

- Authentication 객체가 저장되는 보관소로 필요 시 언제든지 Authentication 객체를 꺼내어 쓸 수 있도록 제공되는 클래스
- ThreadLocal에 저장되어 아무곳에서나 참조가 가능하도록 설계함
- 인증이 완료되면 HTTPSession에 저장되어 어플리케이션 전번에 걸쳐 전역적인 참조가 가능하다

## SecurityContextHolder

- SecurityContext 객체 저장 방식
    - MODE_THREADLOCAL: 스레드당 SecurityContext 객체를 할당, 기본값
        - 자식 쓰레드와는 공유가 되지 않는다. 공유를 하려면 아래 모드로 변환을 해야한다
    - MODE_INHERITABLETHREADLOCAL: 메인 스레드와 자식 스레드에 관하여 동일한 SecurityContext를 유지
    - MODE_GLOBAL: 응용 프로그램에서 단 하나의 SecurityContext를 저장한다
- 객체 저장 방식은 보안 설정 클래스에서 변경할 수 있다.(앱 기동시 초기화)
- SecurityContextHolder.clearContext(): SecurityContext 기존 정보 초기화하는 코드