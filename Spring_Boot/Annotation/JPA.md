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

#### @OnDelete

CascadeType의 속성은 여러가지(ALL, REMOVE, PERSIST)가 있는데 ALL로 쓰면 모든 속성을 사용한다.
그래서 @OnDelete와 CascadeType.ALL은 둘 다 Cascade의 DELETE 기능을 갖고 있다.
즉, 기본키 데이터를 삭제할 경우 외래키 데이터도 전부 삭제된다.
다만 @OnDelete는 프로그램 올라오기 전에 DB 테이블에 접근해서 위처럼 ON DELETE CASCADE를 붙여주는 것이다. 
CascadeType.ALL 기능은 테이블을 바꾸는 것이 아니라 런타임 도중에 JPA에 의해 실행되도록 만드는 것이다.

JPA는 @OnDelete에 표준화되어있지 않다.
JPA 자체가 데이터베이스 저장소에 구애받지 않고(storage-agnostic) 사용 하기 위한 기술이기 때문에 테이블을 변경하는 것을 바람직하지 못하다고 여긴다.
결론은 @OnDelete는 지양하자.

```java

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "sender_id")
    @OnDelete(action = OnDeleteAction.NO_ACTION)
    private User sender;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "receiver_id")
    @OnDelete(action = OnDeleteAction.NO_ACTION)
    private User receiver;

```