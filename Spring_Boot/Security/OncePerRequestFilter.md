![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fryt33%2FbtrUWSzQ8B7%2FUyJhUb6DqYeYkP1MikzlBk%2Fimg.png)

우선 Filter은 DispatchServlet 앞에서 동작하는 큰 문이고, Interceptor은 DispatchServlet과 Controller 사이에서 동작하는 작은 문이다.

#### 필터
- Web Application Context의 기능
- 인코딩, Cors, Log, 인증, 권한 들을 구현

#### 인터셉터
- 스프링에서 SPring Context의 기능 Bean같은 느낌
- 스프링 컨테이너로 다른 빈을 주입하여 사용하기 용이하다.
- 다른 빈을 활용하여 인증, 권한 등을 구현 할 수 있다.

## OncePerRequestFilter

필터의 동작 방식을 생각해보면 하나의 서블릿을 실행하는 동안 다른 서블릿에 대한 다른 요청이 들어올 수 있다. 
이때 다른 서블릿에도 같은 필터가 존재한다면 그 필터가 또 실행된다. 
그럼 필터가 중복으로 실행 되는데 이는 리소스 낭비를 하는 상황이 발생한다.
OncePerRequestFilter은이를 방지해준다.

해당 요청에 대해서 이 필터는 한 번만 실행 되는것을 지정한다

![](https://blog.kakaocdn.net/dn/bIGzIR/btrUV3hmBRh/IyDKNe12kOFpsN4kRKGnb1/img.png)

공식 문서에서 Single execution per request dispatch, on any servlet container 부분이 눈에 들어온다.

이는 우리가 OncePerRequestFilter을 통해서 동일한 request안에서는 필터링을 한번만 할 수 있게 도와준다.

위의 기능으로 인증, 인가를 거치고 url로 포워딩하면, 포워딩 요청의 인증 인가필터를 다시 거치지 않고 다음 로직을 바로 실행하게 해준다.