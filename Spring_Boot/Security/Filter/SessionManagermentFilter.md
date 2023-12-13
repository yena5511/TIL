## SessionManagermentFilter

`로그인 후 Session과 관련된 작업을 처리`

이 필터는 조건이 현재 세션에 SecurityContext이 없거나 세션이 null인 경우에 동작이 된다.

해당 필터에서는 아래의 세 SessionInfo 등록, SessionFixation, ConcurrentSession을
진행한다.
Register SessionInfo
사용자의 세션 정보가 등록한다.
인증에 성공한 이후에 일반적으로 successHandler등 으로 다음 페이지 (가령 루트페이지)로 이동할텐데,
시큐리티 필터가 세션에 최종적으로 인증에 성공한 인증객체(SecurityContext)를 세션에 저장하는 처리를 응답 직전에 처리 된다.
처리 후 응답으로 해당 리소스로 이동하게 된다.
 
SessionFixation
세션 고정 보호로 인증에 성공한 시점에 새롭게 쿠키가 발급되며,
인증을 시도하기 전에 이전에 쿠키가 삭제되고 새로운 쿠키가 발급되도록 작동한다.
 
ConcurrentSession
UsernamePasswordAuthenticationFilter인증을 진행하며 해당 필터가 동시에 검증된다고 했는데
사용자가 인증에 성공했다면 해당 사용자 계정으로 동시점에 세션이 존재하는지 확인힌다.
 
A라는 사람이 로그인을 하고 후에 B라는 사람이 같은 계정으로 로그인을 했다는 상황을 가정해보다.
먼저, A라는 사람은 성공적으로 인증을 받고 세션에 저장된다.
이후 B라는 사람이 인증을 시도하게 되면 해당 필터에서 동시 세션을 처리하게 되고, 
Spring Security 설정 내역에 따라 세션 처리를 진행한다.
 
이 때, 아래의 두 가지 전략에 따릅니다.
 
✔️ 현재 시점 사용자 인증 X
인증 자체를 실행하지 못하도록 인증관련 예외를 날란다.
 
만약에, 이 시스템이 동일한 계정으로 생성할 수 있는 세션의 개수가 하나라고 지정되어있는 경우, 
현재 사용자 인증 시도를 차단한다.
SessionAuthenticationException를 던진다.
 
✔️ 이전 사용자 인증 만료
현재 사용자는 인증을 계속 사용하고, 이전 사용자의 세션을 만료시킨다.
B 사용자를 인증하며 첫 번째 사용자의 세션을 만료하는 설정을 한다.
이전 계정 세션을 session.expireNow를 false로 바꾸게 되고 이때 A 사용자는 리소스에 접근 시 ConcurrentSessionFilter에 걸려 더 이상 해당 인증을 사용하지 못하게 된다.
 