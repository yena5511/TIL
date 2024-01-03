## 계층화 아키텍쳐 (Layered Architecture)

계층화 아키텍쳐는 일반적으로 널리 사용되는 아키텍쳐이다.
구성되는 계층의 숫자에 따라 N계층 아키텍쳐라고도 한다.
각 레이어는 코드나 컴포넌트의 논리적인 분리이다.
유사한 컴퍼넌트를 한 계층에 배치시킨다.
주요 특징은 계층이 바로 아래에 있는 계층과만 연결된다는 것이다.
또한 어떤 계층이 수정되어도 다른 계층에 영향을 주지 않는다.

`4계층을 가지는 아키텍쳐`
1. Presentation Layer
2. Business Layer
3. Persistence Layer
4. Database Layer
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCRKOb%2FbtsbBcCJBQF%2FOxNSmHZAK3mzHQfZPrhKxk%2Fimg.jpg)
#### Presentation layer : View + Controller

presentation Layer(화면 계층)은 화면에 보여주는 기술을 사용하는 영역이다.
소프트웨어 시스템과의 사용자 상호작용을 담당한다.
view와 controller를 모두 포함한고 있지만, 컨트롤러를 빼서 Controller layer로 떼내기도 한다.
view에 해당하는 layer은 말 그대로 사용자 인터페이스에 불과하다.
표현과 이벤트 처리, 데이터 포맷을 책임을 진다.
controller에 해당하는 layer은 구성 요소 간 처리 흐름을 제어하고 데이터 조작 의뢰, 데이터 변환 및 연산, Exception과 Error 처리한다.

#### Application/Business layer : Service

비지니스 계층은 기능의 요구사항 충족과 관련된 일을 처리하는 도메인 계층이다.
즉, 고객이 원하는 요구 사항을 반영하는 계층으로 고객의 요구사항과 정확히 일치하도록 설계해야하며 메서드 이름은 고객들이 사용하는 용어를 그대로 사용하는 것이 좋다.
Control layer와 Persistence layer를 연결하는 역할을 한다.

#### Persistence layer : DAO(Repository)

영구 데이터를 빼내어 객체화 시키며, 영구 저장소에 데이터를 저장, 수정, 삭제하는 계층이다.
데이터베이스나 파일에 접근하여 데이터를 CRUD하는 계층이며, 이처럼 일반적으로 데이터베이스를 많이 이용하지만 경우에 따라 네트워크 호출이나 원격 호출 등의 기술이 접목될 수 있다.

#### Database Layer : VO

관계형 데이터베이스의 엔티티와 비슷한 개념을 가지는 것으로 데이터 객체를 말한다.