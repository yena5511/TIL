## Builder Pattern

복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다은 표현 결과를 만들 수 있게 하는 패턴을 의미한다.
다시 말해 lOMBOK에서 제공하는 Annotation으로 별도의 Builder Pattern을 구성할 필요없이 생성장에 @Builder를 선언하면 이를 자동으로 구성하는 것을 의미한다.

```java
import lombok.*;

@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class SuccessCode {

    private int status;
    private String code;
    private String message;

    @Builder
    SuccessCode(int status, String code, String message) {
        this.status = status;
        this.code = code;
        this.message = message;
    }

}
// Builder Pattern 사용
SuccessCode successCode = SuccessCode.builder()
                .status(200)
                .message("성공 되었습니다.")
                .code("200")
                .build();
```

@NoArgsConstructor(access = AccessLevel.PROTECTED)
- 파라미터가 없는 생성자를 생성해주는 어노테이션으며 무분별한 객체 생성에 대해서 한번 더 체크 할 수 있는 수단이다.
- 개발자가 실수로 클래스의 필드 중 하나의 필드에 대한 값 설정을 누락하였을 경우 객체는 불완정한 상태가 되어 버린다.

