#### JPA(Java Persistence API)

JPA는 자바 진영에서 OMR(object-Relational Mapping) 기술 표준으로 사용되는 인터페이스 모음이다.
즉, 실제적으로 구현된것이 아니라 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크이다.
JPA를 구현하는 대표적인 오픈소스는 Hibernate가 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSn2Dj%2Fbtq0nPrl873%2FACS7qrimAoVuTS8oriAnv0%2Fimg.jpg)

```
ORM(Object-Relational Mapping)

우리가 일반적으로 알고있는 애플리케이션 class와 RDB(Relational DataBase)의 테이블을 매핑(연결)한다는 뜻이며,
기술적으로는 어플리케이션의 객체를 RDB테이블에 자동으로 영속화 해주는 것이라고 보면된다.
```

#### 장점

- SQL문이 아닌 Method를 통해 DB를 조작할 수 있어, 개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있음.(내부적으로는 쿼리를 생성하여 DB를 조작함. 하지만 개발자가 이를 신경쓰지 않아도 됨)
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어, 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높임
- 객체지향적인 코드 작성이 가능하다. 오직 객체지향적 접근만 고려하면 되기때문에 생상성 증가
- 매핑하는 정보가 class로 명시 되었기 때문에 ERD를 보는 의존도를 낮출 수 있고 유지보수 및 리팩토링에 유리
- 예를 들어 기존 방식에서 MySQL 데이터베이스를 사용하다가 PostgreSQL로 변환한다고 가정해보면, 새로 쿼리를 짜야하는 경우가 생김. 이런 경우에 ORM을 사용한다면 쿼리를 수정할 필요가 없음

#### 단점

- 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우, 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있음
- 복잡하고 무거운 Query는 속도를 위해 별도의 튜닝이 필요하기 때문에 결국 SQL문을 써야할 수도 있음
- 학습 비용이 비쌈

#### PA(Java Persistence API)

- Java 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용하는 인터페이스 모음
- 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스
- 인터페이스 이기 때문에 Hibernate, OpenJPA 등이 JPA를 구현함

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FG5499%2Fbtq0iXcSD1z%2FjpJ3duprzK1QCJwijyuhkK%2Fimg.png)

#### 사용하는 이유

JPA는 반복적인 CRUD SQL을 처리해준다.
JPA는 매핑된 관계를 이용해서 SQL을 생성하고 실행하는데, 개발자는 어떤 SQL이 실행될지 생각만하면 되고, 예측도 쉽게 할 수 있다.
추가적으로 JPA는 네이티브 SQL이란 기능을 제공해주는데 관계 매핑이 어렵거나 성능에 대한 이슈가 우려되는 경우 SQL을 직접 작성하여 사용할 수 있다.


JPA를 사용하여 얻을 수 있는 가장 큰 것은 SQL아닌 객체 중심으로 개발할 수 있다는 것이다.
이에따라 당연히 생산성이 좋아지고 유지보수도 수월하다.
또한 JPA는 패러타임의 불일치도 해결하였다.
예를 들면 JAVA에서는 부모클래스와 자식클래스의 관계 즉, 상속관계가 존재하는데 데이터베이스에서는 이러한 객체의 상속관계를 지원하지 않는다(상속 기능을 지원하는 DB도 있지만 객체 상속과는 다름). 
이런 상속관계를 JPA는 아래와 같은 방식으로 해결하였다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcQBTLI%2Fbtq0SkrYdTr%2Fm7EgkroBjYnGKKVkf0LPn0%2Fimg.png)