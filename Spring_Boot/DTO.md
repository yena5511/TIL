## DTO

DTO(Data Transfer Object)란 계층간 데이터를 교환을 위해 사용하는 객체이다.
Client가 Controller에 요청을 보낼 때도 RequestDto의 형식으로 데이터가 이동하고, Controller가 Client에게 응답을 보낼 때도 ResponseDto의 형태로 데이터를 보내게 된다.

#### MVC 패턴

![](https://velog.velcdn.com/images%2Fminide%2Fpost%2F29966859-4aca-4b35-b238-f9aabf5dfe68%2F300px-Router-MVC-DB.svg.png)

MVC 패턴은 애플리케이션 개발 시 그 구셩요소를 Model과 view, Controller 세 가지 역할로 구분하는 디자인 패턴이다.
비지니스 처리 로직(Model)과 UI영역 (View)의 중간에서 Controller가 연결해주는 역할을 한다.

Controller는 View로부터 들어온 사용자 요청을 해석해 Model을 업데이트하거나 Model로부터 데이터를 받아 View로 전달하는 작업 등을 수행한다.
MVC 패턴의 장점은 Model과 View를 분리함으로써 서로의 의존성을 낮추고 독립적인 개발을 가능하게 한다.

Controlller는 View와 도메인 Model의 데이터를 주고 받을 때 별도의 DTO를 주로 사옹한다.
도메인 객체를 View에 직접 전달할 수 있지만, 도메인의 비지니스 기능이 노출될 수 었으며 Model과 View 사이에 의존성이 생기기 때문이다.

#### DTO를 사용하지 않을 경우

`Entity`
```java
public class User {

  private Long id;
  private Sting name;
  private String email;
  private String password;
  ...
}
```

`Controller`
```java
@GetMapping("/page/{id}")
@ResponseStatus(HttpStatus.OK)
public User viewMyPage(@PathVariable("id") Long id) {
    return userService.viewMyPage(id);
}
```
이렇게 Controller가 클라이언트의 요청에 대한 응답으로 도메인 Model인 User를 넘겨주면 다음와 같은 문제점이 있다.

- 도메인 Model의 모든 속성이 외부에 노출된다
    - 위 예시의 경우 사용자가 마이페이지 조회에 대한 요청을 하는데 그에 대한 응답으로 외부에 노출되면 안되는 `password`데이터까지 응답해준다.
    이처럼 도메인 Model 자체를 응답해주면 중요한 정보가 외부에 노출되는 보안 문제가 발생한다.
    - 또한 View에서 보낸 요청에 대한 데이터 외의 불필요한 데이터를 모두 가지고 있다.

- UI 계층에서 Model의 메소드를 호출하거나 상태를 변경시킬 위험이 존재한다.
- Model과 View가 강하게 결합되어, View의 요구사항 변화가 Model에 영향을 끼치기 쉬워진다.
    - 또한 User Entity의 속성이 변경되면, View가 전달박을 JSON 등 클라이언트 코드에도 변경을 유발하기 때문에 상소간 강하게 결합된다.

#### DTO를 사용하는 경우

`UserRespondDto`
```java
public class UserResponseDto {

  private String name;
  private String email;
}
```

`Controller`
```java
@GetMapping("/page/{id}")
@ResponseStatus(HttpStatus.OK)
public UserResponseDto viewMyPage(@PathVariable("id") Long id) {
    return userService.viewMyPage(id);
}
```

DTO를 사용하면 앞에서 언급했던 문제들을 해결할 수 있다. 
도메인 Model을 캡슐화하고, UI 화면에서 사용하는 데이터만 선택적으로 보낼 수 있다.

정리하면 DTO는 클라이언트 요청에 포함된 데이터를 담아 서버 측에 전달하고, 서버 측의 응답 데이터를 담아 클라이언트에 전달하는 계층간 전달자 역할이다.


#### DTO를 사용하는 이유

- View Layer와 DB Layer의 역할을 분리하기 위해서
    - 객체를 표현하기 위한 계층과 저장하는 계층의 역할을 분리하기 위해서 DTO를 사용한다.

- Cntity 객체의 변경을 피하기 위하여
    - Entity 객체를 그대로 사용하면 프로그래머의 의도와 다르게 데이터가 변경될 수 있다.

- View와 통신하는 DTO클래스는 자주 변경된다
    - View(클라이언트)와 통신하는 DTO 클래스, 예를 들어 ResponseDTO, RequestDTO는 요구사항에 따라 자주 변경된다. 
    어떤 요청에서는 특정 값이 추가될 수도 있고, 특정값이 없을 수도 있다. 
    따라서 Entity 클래스와 분리하여 관리해야 한다. 

- 도메인 모델링을 지키기위하여
    - 도메인 설계를 잘하였다고 하더라도 원하는 데이터를 표시하기가 쉽지 않을 수 있다.
    예를 들어 Entity 클래스의 특정 컬럼들을 조합하여 특정 포맷을 출력하고 싶다고 하자. 
    Entity 클래스에 표현을 위한 필드나 로직이 추가되면 객체 설계를 망가뜨릴 수 있다.
    따라서 DTO에 표현을 위한 로직을 추가해서 사용하는 것이 Entity의 도메인 모델링을 지킬 수 있다.