## JPA란 무엇인가

**JPA**

JPA란 자바 진영의 ORM 기술 표준이다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbgJWI9%2FbtrmnnsNi61%2FkvEF6zVYBmy3aKne6AHwi0%2Fimg.png)
JPA는 애플리케이션과 JDBC사이에서 작동한다.

**ORM**

ORM은 이름 그대로 객체와 관계형 데이터베이스를 매핑한다는 뜻이다.
ORM프레임워크는 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해준다.

#### JPA소개

자바 객체 진영은 앤터프라이즈 자바 빈즈라는 기술 표준을 만들았는데 그 안에는 엔터티 빈이라는 ORM 기술도 포함되어 있았다.
하지만 너무 복잡하고 기술 성숙도가 떨어졌으며 자바 엔터프라이즈 애플리케이션 서버에서만 작동했다.

하이버네이트라는 오픈소스 ORM 프레임워크가 등장했는데 EJB의 ORM기술과 비교해서 가볍고 실용적인 데다 기술 성숙도도 높았다.
하이버네이트를 기반으로 새로운 자바 ORM기술 표준이 만들어젺는데 이것이 jPA다.


![](https://velog.velcdn.com/images/2jjong/post/0001540e-933d-4fdd-b009-83b3a243d871/image.jpeg)

JPA는 자바 ORM 기술에 대한 API 표준 명세이다. 
쉽게 이야기해서 인터페이스를 모두 모아둔 것이다. 
따라서 JPA를 사용하려면 구현한 ORM 프레임워크를 선택해야 한다. 
ORM 프레임워크들이 있는데 가장 대중적인 것은 하이버네이트이다.