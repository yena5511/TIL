## CORS (Cross - Origin Resourece Sharing)

- 특정 헤더를 통해 브라우저에게 Origin에서 실행되고  있는 웹 애플리케이션이 Cross-Origin에 리소스에 접근할 수 있는 권한이 있는지 없는지 확인하는 방침이라고 생각하면 편하다
- 웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때  Cross-Origin HTTP 요청을 실행한다

#### Cors요청하는 종류

- XMLHttpRequest와 Fetch API 호출
- 웹 폰트(CSS 내 @font-face에서 교차 도메인 폰트 사용 시)
- WebGL 텍스쳐
- drawImage() (en-US)를 사용해 캔버스에 그린 이미지/비디오 프레임
- 이미지로부터 추출하는 CSS Shapes

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
    - Access-Control-Request-Method: 실제 요청하려는 메소드 종류
    - Access-Control-Request-Headers: 실제 요청에 포함되어 있는 헤더 이름

- Response headers (서버에서의 응답 헤더)
    - Access-Control-Allow-Origin: 요청을 허용하는 출처. * 인 경우, 모든 도메인의 요청을 허용한다.
    - Access-Control-Allow-Methods: 요청을 허용하는 메소드 종류. 헤더 값에 해당하는 메소드만 접근 허용한다. (default : GET, POST)
    - Access-Control-Allow-Headers: 요청을 허용하는 헤더 이름
    - Access-Control-Max-Age: 클라이언트에서 preflight 의 요청 결과를 저장할 시간(sec). 해당 시간동안은 preflight요청을 다시 하지 않게 된다.
