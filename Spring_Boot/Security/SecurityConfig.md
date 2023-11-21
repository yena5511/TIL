## Security Config

```java
  @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .addFilter(corsConfig.corsFilter())
                .addFilterBefore(jwtFilter(), UsernamePasswordAuthenticationFilter.class)
                .csrf().disable()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .formLogin().disable()
                .httpBasic().disable()
                .authorizeRequests()
                .antMatchers("/api/v1/user/**")
                .access("hasRole('ROLE_USER') or hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
                .antMatchers("/api/v1/manager/**")
                .access("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
                .antMatchers("/api/v1/admin/**")
                .access("hasRole('ROLE_ADMIN')")
                .anyRequest().permitAll();
    }
```

- csrf().disable()
: 위변조 방지 미사용 >> 토큰 방식 사용
- cors().configurationSource(corsFilter())
: corsFilter 설정
- sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) /
: session 미사용 >> 토큰 방식 사용
- formLogin().disable()
: 로그인 폼 미사용 >>
UsernamePasswordAuthenticationFilter 미동작 딸로 필터 등록 필요함
formLogin()을 사용하게 되면 서버로 로그인 요청시, username과 password로 인증하는 로직(UserDetailsService)을 수행하고, 세션에 저장한다. >> 
토큰 발급할 것이므로 비활성
- httpBasic().disable()
: 	httpBasic방식 미사용 >> Bearer방식 사용
- authorizeRequests()
: 권한설정 시작
- antMatchers("/api/v1/user/**")	
: 해당 path에 관한 권한 설정
- permitAll()
: 모두 허용
- access("hasRole('ROLE_MANAGER')")	
: MANAGER만 허용

#### httpBasic().disable()

세션방식에서 로그인시, 웹브라우저 쿠키에 세션 ID저장, 요청시 헤더에 쿠키(세션 아이다)를 들고 감

그래서 동일 도메인에서만 허용하도록 httpOnly를 사용 -> ajax나 axios로 요청시, 쿠키를 담아서 보내면 거부하는 기능

httpOnly : http요청이 아니면 쿠키를 건드릴 수 없게함, 아마 SSR으로 추측됨..
http요청이 아닌것 : 스크립트 파일에서.. fetch() . ajax, axios, 이거는 Rest Api 서버로 추측

래서 요청시 인증하기 위해 나온게(as-is: JSESSION? , to-be: Authentication)
인증시 headers.Authorization에 인증정보를 담고가는 2가지 방식이 있다.

1. header에 Authorization에 ID와 패스워드를 담는 방식 >> httpBasic 방식
- 암호화 안됨 중간에 탈퇴 위혐 > https 서버를 사용해야함

2. 토큰을 담는 방식 (JWT)>> Bearer 방식
- 노출이 되면 안되지만 id, password 보다 안전
- 서버쪽에서 계속 새로 만들어줌, 만료 시간 이후 재생성