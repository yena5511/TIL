## UUID

범용 고유 식별자
즉, 식별 가능한 고유값이라는 뜻이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fs4WhE%2Fbtr20z6Jn6S%2FoCWPwQkqukyW1vKuKkomW0%2Fimg.png)

- 일련의 숫자와 문자로 구성된 128비트 값으로 이뤄져 있다.
- 이 값은 거의 중복될 가능성이 없는 매우 낮은 확률로 고유성을 보장받기 때문에 충돌없이 식별자를 생성할 수 있게란다.
- 또한, 전 세계에서 고유한 값으로 취급되기 때문에 여러 시스템간에 데이터를 교환하거나 통합할 때 매우 유용하다.
- UUID는 무작위로 생성되며 시간, 장치의 고유 번호 등 다양한 요소를 기반으로 생성된다.
예측이 불가능하며 보안에도 도움이 된다.



```java
import java.util.UUID;

public class UUIDTest {
    public static void main(String[] args) {

        UUID uuidOne = UUID.randomUUID();
        UUID uuidTwo = UUID.randomUUID();

        System.out.println("UUID 1 => " + uuidOne.toString());
        System.out.println("UUID 2 => " + uuidTwo.toString());
    }
}


//출력
//UUID 1 => sf5e8400-53m2-nn53-me09-456292156231
//UUID 2 => 550e8400-e29b-41d4-a716-446655440000

```

#### UUID VS AUTO INCREMENT

기본키로 UUID를 사용할 경유 얻는 이점

- UUID는 데이터에 대한 정보를 노출하지 않기 때문에 보안상 안전하다.
    - AUTO INCREMENT PK는 키 값이 외부에 노출되기 쉬우며 의도치 않게 정보가 노출될 수 있다.
    - 예를들어 회원 정보를 조회하는 API가 /user/{pk}/와 같은 URL 패턴이라면, 다른 고객의 정보가 쉽게 노출될 수 있다.
    뿐만 아니라 pk로 데이터의 수를 유추할 수 있으므로 비지니스에서 의미가 있는 수치가 노출될 수도 있다.

- 데이터베이스가 여러 개인 경우에도 하나의 UUID는 여러 데이터베이스 중에서도 고유한 값이다.
    - UUID는 독립적으로 개발되는 시스템에서도 유일성을 갖는 범용고유식별자이다.
    - 서로 다은 테이블에서 관리되던 데이터를 하나의 데이터 소스로 합치기 쉽다.
    - ex) A 콘텐츠 테이블이 1개가 있고, 이를 검색 엔진 (ElasticSearch)에 복제하고 있다고 가정하자. 
    잠시 후 B 콘텐츠 테이블이 필요하게 되어, 이 정보를 동일한 ElasticSearch에 추가해야하는 상황이다. 
    만약 A, B 둘 다 숫자 기반의 pk를 사용하고 있었다면 두 콘텐츠의 ID가 충돌나는 현상이 발생하게 된다.
        - 하지만 pk가 UUID랄면 별도로 분리되어 있던 데이터들을 통합해도 문제없다.

기본 키를 UUID로 사영할 경우 단점

- UUID는 INCREMENT PK 보다 더 많은 저장 장소를 필요로한다. (UUID - 128bits)
- 관계 테이블에서 fk로 UUID를 사용한다면, 더 많은 저장 공간을 사용하게 된다.
- 테이블과 인덱스의 크기가 커지므러, DB의 디스크와 메모리를 많이 사용하게 된다.


