## ConcurrentSessionFilter

`동시 세션과 관련된 필터`

현재 사용자 계정으로 인증을 받은 사용자가 두 명 이상일 때 실행되는 필터이다.
 
이미 로그인을 했는데, 같은 계정으로 다른 사람이 로그인을 시도하거나 다른 곳에서 다시 로그인을 시도할 때 세션이 두 개 이상으로 늘어날 수 있다.
ConcurrentSessionFilter는 위의 같은 상황과 같이 동시 세션과 관련된 필터이다.
 
해당 필터는 매 요청마다 현재 사용자가 세션이 만료되었는지 확인한다.
이 때 session.expireNow의 값으로 만료 설정을 구분한다.
이 값은 아래 SessionManagementFilter에서 설정이 된다.

그래서 만약 사용자가 리소스에 접근 중이다가 session.expireNow 값이 true가 되면 
해당 사용자를 로그아웃하고 요청 실패를 던진다.
