## CORS (Cross - Origin Resourece Sharing)

 - HTTP 요청은 기본적으로 Cross-Site HTTP Requests가 가능
- 특정 헤더를 통해 브라우저에게 Origin에서 실행되고  있는 웹 애플리케이션이 Cross-Origin에 리소스에 접근할 수 있는 권한이 있는지 없는지 확인하는 방침이라고 생각하면 편하다
- 웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때  Cross-Origin HTTP 요청을 실행한다
- 프로토콜, 호스트명, 포트가 같아야만 요청이 가능하다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdqfgVS%2FbtqEAC0uJ1H%2FAE9yGoQt975Cz7GGNqJiM1%2Fimg.png)

#### Cors요청하는 종류

- CORS요청의 경우 Simple/Preflight, Credential/Nom-Credential 4가지의 요청이 존재한다.
브라우저가 요청 내용을 분석하여 4가지 방식붕 해당하는 방식을 서버에 요청을 보내기 때문에,
프로그래머가 목적에 맞는 방식을 선택하고 그 조건에 맞게 구현해야 한다.

- Simple Request
	- 아래의 조건을 모두 만족하면 Simple Request
	- GET, POST, HEAD 중의 한가지 Method를 사용
	- POST 방식일 경우에는 Content-type이 셋 중에 하나여야 한다.
		1.  application/x-www-form-urlencoded
		2.  multipart/form-data
		3.  text/plain
	-  Custom Header를 전송하면 안된다.

Simple Request경우 서버에 1회 Request, 서버도 1회 Response 하는것으로 처리가 종료된다.

-  Preflight Request
	- Simple Request 조건에 해당하지않을때 Request방식 이 요청방식은 예비 요청과 본 요청으로 나누어서 전송하게 된다.
	 - 따라서 브라우저는 예비요청을 보내고 응답받고 본요청을 보내고 응답받고 2번의 처리가 이루어지게 된다.

하지만 예비요청과 본요청에대한 서버단의 응답을 프로그래머가 직접 프로그램내에서 구분해서 처리하지 않는다.
Access-Control- 계열의 Response Header만 적절하게 정해주면, OPTIONS 요청으로 오는 예비 요청 과 POST, GET, HEAD, PUT ,DELETE로 오는 본요청의 처리는 알아서 이루어지게 된다.


#### Cors Filter 적용

```java
import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletResponse;

public class CORSFilter implements Filter {
	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException { 
		HttpServletResponse response = (HttpServletResponse) res; 
		response.setHeader("Access-Control-Allow-Origin", "*"); 
		response.setHeader("Access-Control-Allow-Methods", "POST, GET, DELETE, PUT"); 
		response.setHeader("Access-Control-Max-Age", "3600"); 
		response.setHeader("Access-Control-Allow-Headers", "x-requested-with, origin, content-type, accept"); 
		chain.doFilter(req, res); 
		} 
	public void init(FilterConfig filterConfig) {} 
	public void destroy() {}
}
 
```

- Request headers (클라이언트의 요청 헤더)
    - Origin: 요청을 보내는 페이지의 도메인
	(허용할 URL)
    - Access-Control-Request-Method: 실제 요청하려는 메소드 종류
    - Access-Control-Request-Headers: 실제 요청에 포함되어 있는 헤더 이름

- Response headers (서버에서의 응답 헤더)
    - Access-Control-Allow-Origin: 요청을 허용하는 출처. * 인 경우, 모든 도메인의 요청을 허용한다.
	(허용할 Header)
    - Access-Control-Allow-Methods: 요청을 허용하는 메소드 종류. 헤더 값에 해당하는 메소드만 접근 허용한다. (default : GET, POST)
    - Access-Control-Allow-Headers: 요청을 허용하는 헤더 이름
    - Access-Control-Max-Age: 클라이언트에서 preflight 의 요청 결과를 저장할 시간(sec). 해당 시간동안은 preflight요청을 다시 하지 않게 된다.

- addAllowedMethod() 
	- 허용할 Http Method
