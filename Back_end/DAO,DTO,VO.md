## 스프링 패키지 구조

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJRXYu%2Fbtry2S3eRZK%2FSgDyyDlB7XHSN5qMDyEr11%2Fimg.webp)

스프링 패키지를 나눌때 전체적인 그림은 일반적으로 위 사진과 같다. 
이렇게 계층을 나누어 관리하는 것이 유지 보수하기에 용이하며 디버깅하기도 쉽다.

## DTO(Data Transfer Object)

- DTO는 말 그대로 데이터를 Transfer(이동)하기 위한 객체이다.
- Client가 Controller에 요청을 보낼 때도 RequestDto의 형식으로 데이터가 이동하고, Controller가 Client에게 응답을 보낼 때도 ResponseDto의 형태로 데이터를 보내게 된다.
- Controller와 Service, Repository 계층 사이에 데이터가 오갈 때도 데이터는 DTO의 형태로 이동하게 된다. 
- DTO는 로직을 갖고 있지 않는 순수한 데이터 객체이며, 일반적으로 getter/setter 메서드만을 가진다. 
- 하지만 DTO는 단순히 데이터를 옮기는 용도이기 때문에 굳이 Setter를 이용해 값을 수정할 필요가 없이, 생성자만을 사용하여 값을 할당하는 게 좋다.

#### DTO를 사용하는 이유

- . View Layer와 DB Layer의 역할을 분리하기 위해서
    - 객체를 표현하기 위한 계층과 저장하는 계층의 역할을 분리하기 위해서 DTO를 사용한다.
- Entity 객체의 변경을 피하기 위해
    - Entity 객체를 그래도 사용하면 프로그래머의 의도와 다르게 데이터가 변질될 수 있다.
- View와 통신하는 DTO 클래스는 자주 변경된다.
    - View(클라이언트)와 통신하는 DTO 클래스, 예를 들어 ResponseDTO, RequsetDTO클래스는 요구사항에 따라 자주 변경된다.
    어떤 요청에서는 특정 값이 추가될 수 있고, 특정값이 없을 수도 있다.
    따라서 Entity 클래스와 분리되어 관리해야 한다.
- 도메인 모델링을 지키기 위하여
    - 도메인 설계를 잘하였더라도 원하는 데이터를 표시하기가 쉽지않을 수 있다.
    예를 들어 Entity 클래스의 특정 컬럼들을 조합하여 특정 포맷을 출력하고 싶다고 하자.
    Entity클래스에 표현을 위한 필드나 로직이 추가되면 객체 설계를 망가뜨릴 수 있다.
    따라서 DTO에 표현을 위한 로직을 추가해서 사용하는 것이 Entity의 도메인 모델링을 지킬 수 있다.

## DAO(Data Access Object)

- DAO는 말 그대로 실제 DB에 접근하는 객체를 뜻한다. 
- DAO는 Service와 실제 데이터베이스를 연결하는 역할을 하게 된다. 
즉, DB에서 데이터를 꺼내오거나 넣는 역할을 DAO가 담당한다.
- JPA의 경우 Repository가 DAO의 역학을 한다고 볼 수 있다.
하지만 DOA와 Repository가 같은 것은 아니다.

#### DOA와 Repository의 차이점

DAO와 REPOSITORY 모두 퍼시스턴스 로직에 대한 객체-지향적인 인터페이스를 제공하고 도메인 로직과 퍼시스턴스 로직을 분리하여 관심의 분리(separation of concerns) 원리를 만족시키는데 목적이 있다. 
그러나 비록 의도와 인터페이스의 메서드 시그니처에 유사성이 존재한다고 해서 DAO와 REPOSITORY를 동일한 패턴으로 취급하는 것은 성급한 일반화의 오류를 범하는 것이다.
DAO는 퍼시스턴스 로직인 Entity Bean을 대체하기 위해 만들어진 개념이다. 
DAO가 비록 객체-지향적인 인터페이스를 제공하려는 의도를 가지고 있다고 하더라도 실제 개발 시에는 하부의 퍼시스턴스 메커니즘이 데이터베이스라는 사실을 숨기려고 하지 않는다. 
DAO의 인터페이스는 데이터베이스의 CRUD 쿼리와 1:1 매칭 되는 세밀한 단위의 오퍼레이션을 제공한다.
반면 REPOSITORY는 메모리에 로드된 객체 컬렉션에 대한 집합 처리를 위한 인터페이스를 제공한다.
DAO가 제공하는 오퍼레이션이 REPOSITORY 가 제공하는 오퍼레이션보다 더 세밀하며, 결과적으로 REPOSITORY에서 제공하는 하나의 오퍼레이션이 DAO의 여러 오퍼레이션에 매핑되는 것이 일반적이다. 
따라서 하나의 REPOSITORY 내부에서 다수의 DAO를 호출하는 방식으로 REPOSITORY를 구현할 수 있다.

## VO

VO(Value Oject)는 값 그 자체를 표현하는 객체이다.
객체의 정보가 변경되지 않는 '불변성'을 보장한다.

- VO 내부에 선언된 속성(필드)의 모든 값들이 VO객체마다 값이 같아야 똑같은 객체라고 판별한다.
- 서로 다른 이름을 갖는 VO 인스턴스라도 모든 속성 값이 같다면 두 인스턴스는 같은 객체로 인식
- VO에서 객체를 속성 값만으로 비교하도록 Object클래스의 `equals()`와 `hashcode()`를 오버라이딩해 구현한다.
- getter와 setter을 가질 수 있으며, 그 외의 로직도 가질 수 있음(테이블 내 있는 속성 외 추가적인 속성을 가질 수 있음)

ex) 만원짜리의 지폐의 고유번호가 달라도 같은 만원으로 보는 것처럼 "값" 자체로만 비교를 한다.

```java
public class Money{
	private final int value;
    
    public Money(int value) { // 생성자
    	this.value = value;
    }
    
    public int getHalfValue(){ // setter/getter 외의 로직 추가 가능
    	return value/2;
    }
    
    // equals()와 hashcode()를 오버라이딩
    @Override
    public boolean equals(Object o) {
    	if(this == o) return true;
        if(!(o instanceof Money)) retrun false;
        Money money = (Money) o;
        return value == money.value;
    }
    
    @Override
    public boolean hashCode() {
    	return Objects.hash(value);
    }
}
```

#### VO vs DTO

둘 다 데이터를 담고있는 객체지만 차이점이 있다.

- VO의 경우 Read only의 목적이 강하고 데이터 자체도 불변하게 설계하는 것이 정석이다.
- DTO는 주로 데이터 수집의 용도가 좀 더 강하자.

ex) 웹 화면에서 로그인하는 정보는 `DTO`로 처리하고, 테이블과 관련된 데이터는 `VO`로 처리한다.



