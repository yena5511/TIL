## Servlet

![](https://velog.velcdn.com/images/oyoungsun/post/4ee340ab-2601-41ab-a499-ae09e32c5ec2/image.png)

#### 서블릿의 정의
: 클라이언트의 요청을 처리하고 그 결과를 반환하는 기술

- 동적 웹페이지 서버에서 수행되는 소형 프로그램이다
- 각 사용자의 요청이 서버 하나의 스레드로 수행된다
'자바 서블릿'은 자바를 사용해 웹 페이지를 동적으로 생성하는 서버 내 프로그램이라고 한다
- JSP와 달리 자바코드 안에 HTML을 포함하고 있다는 점이 다른다
(JSP는 Html에 자바 코드 포함)

1. 외부 요청에 대해 스레드로 응답하기 때문에 프로세스로 응답하는 경우보다 가볍다
2. java로 개발되어 다양한 플랫폼에서의 동작이 가능하다

![](https://velog.velcdn.com/images/oyoungsun/post/3f7e0429-9ba0-4bcd-b44a-7efb0a52f185/image.png)

ex) 로그인 페이지
- Http Request에 사용자의 Id/Password가 들어간다면
- Http Response로는 로그인 후 페이지를 보내야 한다
- 사용자의 로그인 정보를 받아 확인후, 다음 페이지를 보내는 프로그램이 바로 서블릿이다
- 웹서버는 요청을 WAS(웹 어플리케이션 서버)에게 넘기고, WAS는 요청에 따른 서블릿을 실행한다
서블릿은 요청에 대한 응답을 클라이언트에게 보낸다

#### 서블릿의 특징

- 웹 서버가 동적인 페이지를 제공할 수 있도록 돕는다
- html을 사용하여 요청에 응답한다
- Java Thread를 이용하여 동작한다
- MVC 패턴에서 Controller로 이용된다
- HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속 받는다
- UDP보다 철리 속도가 느리다
- HTML 변경 시 Servlet을 재 컴파일해야 하는 단점이 있다
- 서블릿은 서블릿 컨테이너에서 관리된다(new로 생성되지 않으며, main()이 없다)

![](https://velog.velcdn.com/images/oyoungsun/post/4f93ddfb-c949-4fac-9f4d-ca2d99f99f00/image.png)

1. 사용자(클라이언트)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송한다
2. 요청을 전송받은 Servlet Cotainer는 HttpServletRequest, HttpServletResponse객체를 생성한다
3. web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾는다
4. 해당 서블릿에서 service메서드를 호출한 후 클라이언트의 GET,POST여부에 따라 doGet()또는 doPost()를 호출한다
5. doGet() or doPost()메소드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보낸다
6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킨다