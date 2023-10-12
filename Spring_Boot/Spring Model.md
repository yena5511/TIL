## Spring Model 객체

Model은 스프링이 지원하는 기능으로써, key와 value로 이루어져있는 HashMap이다<br>
Controller의 메서드는 Model이라는 타입의 객체를 파라미터로 받을 수 있다<br>
순수하게 JSP Servlet으로 웹 어플리케이션을 만들 때 보통 request이나 session 내장객체에 정보를 담아 jsp에 넘겨주곤 했는데 Spring에서는 Model 이라는 것을 쓴다<br>
즉 `requst.setAttribute()`와 비슷한 역할을 하는 것


`jsp로 게시판을 만들 때 많이 사용하는 코드 형태`
```java
request.setAttribute("time", new java.util.Date());
RequestDispatcher dispatcher = request.getRequestDispatcher("url");
dispatcher.forward(request, response);
```
- 스프링에서는 다음과 같이 처리한다

```java
public String home(Model model) {
	model.addAttribute("time", new java.util.Date());
    	return "home";
}
```
- 개발자가 직접 model을 생성할 필요는 없다
- 파라미터로 선언만 해주면 스프링이 알아서 만들어 준다

스프링 MVC의 Controller는 기본적으로 Java Beans 규칙에 맞는 객체는 자동으로 화면에 전달해준다
Java Beans의 규칙에 맞는다는 것은 단순히 생성자가 없거나 빈 생성자를 가지며, getter/setter를 가진 클래스의 객체들을 의미한다
전달될 때는 클래스명의 앞글자를 소문자로 처리하여 전달한다

그러나 기본 자료형(int, double등등)은 파라미터로 선언되었더라도 화면에 자동으로 전달되지 않는다

`Controller의 메소드`
```java
@GetMapping("/ex04")
public String ex04(SampleDTO dto, int page) {
	return "ex04"
}
```
- SampleDTO는 string name, int age를 인스턴스 변수로 가진다

`데이터를 출력하는 코드`
```java
<h2>Sample DTO : ${sampleDTO}</h2>
<h2>page : ${page}</h2>
```

브라우저의 주소창을 통해 쿼리스트링을 `name=aaa&age=11&page=99`와 같이 전달해보면 출력되는 결과화면에는 page는 뜨지 않는다

기본 자료형은 어떻게 view로 전달될까?
- 파라미터에 Model타입의 객체를 선언. 이후 addAttribute()를 통해 전달
- @ModelAttribute 어노테이션 사용
```java
@GetMapping("/ex04")
public String ex04(SampleDTO dto, @ModelAttribute("mypage") int page) {
	return "ex04"
}
```

기본 자료형인 파라미터 앞에 붙여주면 된다
속성값으로는 화면에서 출력할때 사용할 이름을 지정해준다

```
<h2>page : ${mypage}</h2>
```

#### RedirectAttributes

`RedirectAttribues`: 타입의 객체는 일회성으로 데이터를 전달할 수 있다`response.sendRedirect()`와 동일한 용도로 사용이 가능한 객체이다

`addFlashAttribute()` 메서드는 (이름, 값)을 파라미터로 이용하여 화면에 딱 한번만 사용하고 사라지는 데이터를 전달한다
새로고침하면 날라간다
`addAttribute()`리디렉트할 주소 뒤에 쿼리스트링으로 데이터를 전달해준다

#### Controller 메서드의 리턴 타입

- String: jsp를 이용하는 경우 jsp파일의 이름을 나타냄
- void: 호출하는 URL과 동일한 이름의 jsp를 나타냄
- VO, DOP타입: 주로 json타입의 데이터를 만들어서 반환하는 용도
- ResponseEntity: response할떄 HTTP헤더 정보와 내용을 가공
- Model, ModelAndView: Model롤 데이터를 반환하거나 화면까지 지정
- HttpHeaders: 응답에 내용없이 HTTP헤도 메시지만 전달하는 용도로 사용