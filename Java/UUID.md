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
출처: https://hajoung56.tistory.com/88 [무사뎀벨레의 블로그:티스토리]
```