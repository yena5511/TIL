## Mediator Pattern(중재자 패턴)

객체간의 상호작용을 갭슐화하는 디자인 패턴

클래스간의 상호작용하는 커뮤니케이션이 많아 질수록 유지보수나 리팩토링이 어려워진다.
또한 하나의 클래스에서 코드를 변경한다면 다른 코드에도 영향을 미친다.

중재자 패턴은 중재자라는 객체 안에서 서로 다른 객체들을 캡슐화하여 객체들이 더 이상 직접적으로 상호작용하지 않고 중재자를 통해서만 커뮤니케이션하도록 한다.

#### 구조

![](https://velog.velcdn.com/images/wlsrhkd4023/post/a735efc4-bd0b-467b-b1c1-9d25d60f5be0/image.jpg)

- Mediator: Colleague객체간의 커뮤니케이션을 위한 인터페이스 정의
- Colleague: Meditor를 통해 다른 Colleague들간의 상호 커뮤니케이션을 위해 Colleague들을 가지고 있으며 커뮤니케이션을 조정함
- ConcreateMediator: Mediator구현체로 Colleague들간의 상호 커뮤니케이션을 위해 Colleague들을 가지고 있으며 커뮤니케이션을 조정함
- ConcreteColleague: Colleague인터페이스 구현체

#### 예시

![](https://velog.velcdn.com/images/wlsrhkd4023/post/e8267cba-6920-4002-a3d0-11e931ee57b2/image.png)

#### 장점

-  단일 책임 원칙
    - 다양한 컴포넌트 간의 통신을 한곳으로 추출하여 코드를 이해하고 유지 관리하기 쉽게 만들 수 있다.
- 개방/폐쇄 원칙
    - 실제 컴포넌트들을 변경하지 않고도 새로운 중재자들을 도입할 수 있다.
- 프로그램의 다양한 컴포넌트 간의 결합도를 줄일 수 있다.
- 개별 컴포넌트들을 더 쉽게 재사용할 수 있다.