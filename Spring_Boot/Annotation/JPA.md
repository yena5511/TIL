## JPA Annotation

jpa를 사용하면 DB 데이터에 작압할 경우 실제 쿼리를 사용하지 않고 Entity 클래스의 수정을 통해 작업한다.

#### @Entuty

실제 DB의 테이블과 매칭된 class임을 명시한다.
즉, 테이블과 링크될 클래스임을 나타낸다.

#### Entity Class

가장 Core한 클래스로 클래스 이름을 언더스코어 네이밍(_)으로 테이블 이름을 매칭한다.

`SalesManage스.java -> sales_manager table`

#### Controller에서 쓸 DOT 클래스란?

Request와 Response용 DTO는 View를 위한 클래스로, 자주 변경이 필요한 클래스이다.
Entity 클래스와 DTO 클래스를 분리하는 이유는 View Layer와 DB Layer를 철저하게 역할 분리하기 위해서다.

테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO 클래스(Request/ Response 클래스)는 자주 변경되므로 분리해야 한다.

#### @Table

Entity Class에 매핑할 테이블 정보를 알려준다.
`@Table(name = "USER")`
Annotation을 생략하면 Class 이름을 테이블 이름 정보로 매핑한다.

#### @ID
해당 테이블의 PK 필드를 나타낸다

#### @GeneratedValue

PK의 생성 규칙을 나타낸다.
가능한 Entity의 PK는 Long 타입의 Auto_increment를 추천
스프링 부트 2.0에선 옵션을 추가하셔야만 auto_increment가 된다.

기본값은 AUTO로, MySQL의 auto_increment와 같이 자동 증가하는 정수형 값이 된다.

#### @Column

테이블의 컬럼을 나타내며, 굳이 선언하지 않더라도 해당 class의 필드는 모두 컬럼이 된다.
@Column을 생략하면 필드명을 사용해서 컬럼명과 매핑

`@Column(name = "username")`
`
@Column을 사용하는 이유는, 기본값 외에 추가로 변경이 필요한 옵션이 있을 경우 사용한다.

문자열의 경우 VARCHAR(255)가 기본값인데, 사이즈를 500으로 늘리고 싶거나(ex: title),
타입을 TEXT로 변경하고 싶거나(ex: content) 등의 경우에 사용

