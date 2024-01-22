## Java Beans Pattern(자바 빈즈 패턴)

자바빈즈 패턴은 매개변수가 없는 생성자로 객체를 만든후, setter 메서드를 멤버 필드의 값을 설정하는 방법이다.

`생성자 패턴`
```java
import lombok.*;

@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class SuccessCode {
    private int status;
    private String code;
    private String message;
}

```

#### 장점

클라이언트 코드의 가독성이 좋다.
setter 메서드의 이름에 멤버 필드의 이름이 포함되어 있기 때문에, 이떤 멤버 필드에 어떤 값이 들어가는지 쉽게 파악할 수 있다.

#### 단점

객체의 필수 멤버 필드에 값이 들어가기 전까지는 객체가 불완전한 상태이다.
기본 생성자로 객체를 생성한 직후에는 멤버 필드에 빈 값이 들어가게 된다.
객체 하나를 만들기 위해 여러 메서드가 호출된다.
생성자 하나면 호출하면 점층적 패턴과 달리, 자바빈즈 패턴은 여러 setter 메서드를 호출한다.
매개변수가 많아질 수록 더 많은 메서드를 호출해야 한다.
