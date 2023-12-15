## ExceptionTranslationFilter
`필터 체인내에서 발생되는 인증, 인가 예외를 처리`
 
인증, 인가 예외가 발생할 경우 실행된다.
인증과 인가에 대해 각각 AccessDeniedException, AuthenticationException를 던진다.
 
chain.doFilter 로 바로 다음 필터로 넘기는 것을 try, catch로 감싸서 예외를 처리한다.


## FilterSecurityIntrerceptor

`권한 부여와 관련한 결정을 AccessDecisionManager에게 위임해 권한부여 결정 및 접근 제어 처리`

webSecurityConfigurerAdapterd을 구현한 설정 파일에 리소스 접근에 대한 설정을 했다.
그 떄 실행했던 리소스 관련 설정들이 바로 HttpResourceSecurityConfig이며, 해당 필터에서 그 설정들을 처리하게 된다.

#### Chrck Authrnticated

인증 객체가 존재하는지 확인한다.
인증 객체가 없으면 즉시 인증 예외를 날리는데, 인증 객체가 없으면 인가 판단이 불가능하기 때문이다.
안증 개체가 있다면, 아래의 동작을 수행한다.

#### AccessDecisionManager

Security의 인가는 AccessDecisionManager가 담당한다.
 
인가 처리를 하며 AccessDecisionManager가 내부적으로 아래(accessDecisionVoter) 실행한다.
Voter들을 사용하며 인가에 사용되는 여러 전략들이 존재한다.
권한 없으면 AccessDeniedException을 날린다.
 
#### accessDecisionVoter
접근하고자하는 자원의 승인과 거부를 판단한다.

