## RememberMeAuthenticationFilter

`세션이 사러지거나 만료 되더라도, 쿠키 또는 DB를 사용하여 저장된 토큰 기반으로 인증을 처리 `

RememberMe 기능을 활성하고 인증받고 세션이 만료되면 실행되는 필터이다.

세션이 만료되거나 무효화되어서 세션안에 있는 SecurityContext내의 인증 객체가 null일 경우 해당 필터가 작동한다.
인증 객체가 null일 경우에 현재 사용자가 요청하는 requset header애 remember-me cookie 값을 헤더에 저장한 상태로 왔을 때 이 터가 접속한 사용자 대신에 인증처리를 시도한다.

## AnonymousAuthenticationFilter

`사용자 정보가 인증되지 않았다면 익명 사용자 토큰을 반환`

익명 사용자 필터는 이 필터가 호출되는 시점까지,
인증 시도를 하지 않고 어떤 자원에 바로 접속을 시도하는 경우 살행된다.

이 필터는 인증되지 않은 사용자가 접근했을 때 annonymouseAuthenticationToken을 만들어서 SecurityContext 객체에 저장하는 역할을 한다.