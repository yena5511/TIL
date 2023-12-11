## UsernamePasswordAuthenticationFilter

`ID와 Password를 사용하는 실제 Form 시반 유저 인증을 처리`

인증객체를 만들어서 AUthentication 객체를 만들어 아이디 패스워드를 저장하고,
AuthenticationManager에게 인증 처리를 맏긴다.

Authentication
Authentication겍체는 인증 시 id와 password를 담고 인증 검증을 위해 전달되어 사용된다.
인증 후 촤종 인증 결과 (user 객체, 권한정보)를 담고 SecurityContext에 저장되어 전역적으로 참조가 가능한다.
사용자의 인증 정보를 저장하는 토큰 개념이다.

AuthenticationManager가 실직적인 인증을 검증 단계를 총괄하는 클래스인 AuthenticationProvider에게 인증 처리를 위임한다.
그럼 AuthenticationProvider가 UserDetailsService와 같은 서비스를 사용해서 인증을 검증한다.

최종적으로 인증을 성공한 경우, 인증에 성공한 결과를 담은 인증객체를 생성한 다음에 SecurityContext에 저장한다.
이 떄, SecurityContextHolder 안에 있는 SecurityContext는 SecurityContextPeristenceFilter에서 생성하고 저장한 객체를 참조한다.

인증 후 후속처리를 할텐데, 이 때  SessionManagementFilter가 가진 Register SessionInfo, SessionFixation, Concurrentsession을 인증을 시도하는 당시 동시에 진행한다.