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

|속설|기능|
|:---|:---|
|nama|필드와 매핑할 테이블의 컬럼 이름 지정/default는 필드이름으로 대체|
|insertable|true : 엔티티 저장시 필드값 저장/false : 필드값이 저장되지 않음|
|updatable|true : 엔티티 수정시 값이 수정/false : 엔티티 수정시 값이 수정 되지 않음|
|table|하나의 엔티티를 두 개 이상의 테이블에 매핑할 때 사용
|
|nullable|null값 허용 여부 설정/false : not null 제약 조건|
|columnDefinition|데이터베이스 컬럼 정보를 직접 부여|
|length|문자 길이 제약조건/String 타입일 때 사용|
|precision, scale|BigDecimal 타입에서 사용/precision : 소수점을 포함한 전체 자릿수 설정/scale : 소수의 자릿수|

`insertable`
```java
//Entity
@Entity(name="user2")
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @Id
    @GeneratedValue
    private Long id;
    @Column(insertable = false)
    private String name;
    private String age;
}

//Service
@Service
@RequiredArgsConstructor
public class TestService {
    private final EntityManager em;
    @Transactional
    public void test() {
        User user = User.builder()
                .age("12")
                .name("test")
                .build();
        em.persist(user);
    }
}

```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fepwbne%2FbtqXwcb9yL4%2F19j7ggtEWosKeYZqD7AaD1%2Fimg.png)
위의 결과는 User 엔티티에 name 컬럼에 "test"를 입력해도 DB에는 값이 들어가지 않는다.

`updatable`
```java
//Entity
@Entity(name="user2")
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Setter
public class User {
    @Id
    @GeneratedValue
    private Long id;
    @Column(updatable = false)
    private String name;
    private String age;
}

//Service
@Service
@RequiredArgsConstructor
public class TestService {
    private final EntityManager em;
    @Transactional
    public void test() {
        User user = User.builder()
                .age("12")
                .name("test")
                .build();
        em.persist(user);
        user.setName("change test");
    }
}

```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbfkAPo%2FbtqXj7iJr7U%2FU4eqMNQ5AsUbDLIkFBG180%2Fimg.png)

위의 결과는 User 엔티티 name 컬럼에 "test"를 입력하고 "change test"로 변경해도 변경값이 적용되지 않는다.

`nullable`
```java
@Entity(name="user2")
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Setter
public class User {
    @Id
    @GeneratedValue
    private Long id;
    @Column(nullable = false)
    private String name;
    @Column(nullable = true)
    private String age;
}

```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDzQ2J%2FbtqXu5xA0rp%2Fbqt1TNaS5OrIUckiovLKX1%2Fimg.png)
위의 결과는 nullable true, false에 따라 not null이 적용되는지 여부이다.

`unique`
```java
@Entity(name="user2")
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Setter
public class User {
    @Id
    @GeneratedValue
    private Long id;
    @Column(unique = true)
    private String name;
    @Column(unique = false)
    private String age;
}
```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUFcKm%2FbtqXDfr9iP8%2FLN2SqTjUVFYhocHSua4B81%2Fimg.png)

`columnDefinition
`
```java
@Entity(name="user2")
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Setter
public class User {
    @Id
    @GeneratedValue
    private Long id;
    @Column(unique = true)
    private String name;
    @Column(columnDefinition = "VARCHAR(15) NOT NULL")
    private String age;
}
```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb7sOsv%2FbtqXDgdwfsX%2F8aVgqKyTKCA9zMg1a7f6d1%2Fimg.png)


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

#### Access

@Access는 JPA가 엔티티 데이터에 접근하는 방식을 지정한다.

|접근 방식|기능|
|:---|:---|
|AccessType.FILED|필드에 직접 접근/필드 접근 권한이 private여도 접근 가능|
|AccessType.PROPERTY|getter를 통해 접근|

@Access를 설정하지 않으면 기본키를 설정하는 @Id의 위치를 기준으로 접근 방식 설정

```java
@Entity(name = "user2")
@Table(name = "user3")
@Getter
@Setter
public class User {
    @Id
    @GeneratedValue
    private Long id;
}
```

위 코드처럼 @Id가 필드에 설정되면 @Access(AccessType.FILED)로 설정된 아래 코드랑 같다.

```java
@Entity(name = "user2")
@Table(name = "user3")
@Getter
@Setter
@Access(AccessType.FIELD)
public class User {
    @Id
    @GeneratedValue
    private Long id;
}
```

아래 처럼 getter에 @id를 설정함으로 써 프로퍼티 접근으로 할 수 있다. 아래와 같은 경우 @Id가 getter에 위치하므로 @Access를 생략할 수 있다.

```java
@Entity(name = "user2")
@Table(name = "user3")
@Setter
@Access(AccessType.PROPERTY)
public class User {

    @GeneratedValue
    private Long id;

    @Id
    public Long getId() {
        return id;
    }
}
```

필드 접근과 프로퍼티 접근을 혼합하여 아래처럼 사용할 수 있다.

```java
//Entity
@Entity(name = "user2")
@Table(name = "user3")
@Getter
@Setter
public class User {
    @Id
    @GeneratedValue
    private Long id;

    @Transient
    private String name;

    @Access(AccessType.PROPERTY)
    public String getFullName() {
        return name + " hello";
    }
    protected void setFullName(String firstName) { }
}

//Service
@Service
@RequiredArgsConstructor
public class TestService {
    private final EntityManager em;
    @Transactional
    public void test() {
        User user = new User();
        user.setName("aaa");
        em.persist(user);
    }
}
```
getter에 AccessType.PROPERTY를 함으로써 getter의 fullName을 column으로 지정한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fplxwm%2FbtqXD2F717o%2FlkqKuMECkuxoHmksMcvoyk%2Fimg.png)
