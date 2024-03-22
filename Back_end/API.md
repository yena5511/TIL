## API
Application Programming Interface(애플리케이션의 프로그래밍 통신 수단)
-> 컴퓨터 사용할 때. 일상 생활의 거의 모든 것

![](https://velog.velcdn.com/images%2Ftaeha7b%2Fpost%2F60b3813e-6a51-4598-a412-62e4267a7e00%2F%EC%9A%94%EC%B2%AD%EA%B3%BC%20%EC%9D%91%EB%8B%B5.png)

- 클라이언트쪽에서 요청을 하면 서버에서 응답
- 웹 API는 웹 애플리케이션 개발을 할때 클라이언트와 서버, 애플리케이션과 애플리케이션등 서로 요청과 응답을 주고 받기 위해서 정의한 API이다.

- 웹 API의 역할
    - 서버와 데이터베이스안의 리소그에 접근할 수 있게 해준다.
    데이터베이스의 정보를 누구나 열람하면 안되고 필요에 의해서만 열람되어야 함
    API는 접근 권한이 인가된 사람에게만 서버와 데이터베이스에 접근할 수 있게한다.
    - 모든 요청과 응답을 표준화한다.
    애플의 아이폰을 쓰던 삼성으 갤러시를 쓰던 상관없이 동일한 API를 사용하기 때문에 클라이언트의 요청과 서버의 응답을 하나의 API로 표준화 한다.

## CRUD

Create = POST / body
:create는 서버에 정보를 올려달라는 요청, create는 post를 통해 해장 URI를 요청하면 리소스를 생성함

Read = GET / param<br>
:read는 서버에서 정보를 불러오는 요청
GET을 통해 해당 리소스를 조회후 해당 도쿠먼트에 대한 자세한 정보를 사져옴

Update = PUT / body<br>
: update는 정보를 바꾸는 요청

Delete = DELETE / param <br>
:delete는 정보를 지우는 요청
DELETE를 통해 리소스를 삭제할 수 있음

쇼핑몰의 예) <br>
관리자는 상품정보를 "등록(C)/수정(U)/조회(R)/삭제(D)" 할 수 있어야 하며,
 사용자는 상품 정보를 "조회(R)" 또는 상품을 "구매(C - 구매 정보 / U - 재고)"할 수 있어야 한다


`Repository`
Repository의 정확한 사용은 DAO를위해 사용하는 어노테이션인데 jpaRepository는 JPA의 구현제라고 할 수 있다
우리가 JPA의 가장 큰 장점은 데이터의 효울적인 저장과 처리 및 가공이다
그런 데이터의 효율적인 처리를 위해서 Jpa는 기본적인 CRUD가 정의되어 있는 JpaRepository를 구현하였 사용자들의 그 구현체를 상속하여 사용한다