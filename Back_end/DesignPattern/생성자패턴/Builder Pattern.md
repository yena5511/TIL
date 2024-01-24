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

#### 사용하는 이유

- 생성자의 입력값에 대한 순서가 바쓀 경우에 대한 오류를 줄일 수 있다.

점층적인 패턴은 사용하려는 생성자마다 모두 만들어 줘야 하는 문제가 있다.
그리고 또한 매개변수의 개수가 동일한 경우 다른 생성자를 구성할 수 없다는 점이 있다.

`점층적인 패턴`
```java
public class SuccessCode {
    private int status;
    private String code;
    private String message;

    // 매개변수 1개
    SuccessCode(int status) {
        this(status, null, null);
    }

    // 매개변수 2개
    SuccessCode(int status, String code) {
        this(status, code, null);
    }

    // 매개변수 2개
    SuccessCode(String code, String message) {
        this(status, null, message);
    }

    // 매개변수 2개
    SuccessCode(String code, String message) {
        // 해당 부분에 오류 발생!
        this(null, code, message);
    }

```

빌더 패턴의 경우 전체 생성자를 만들어두고 사용하지 않는 값에 대해서 초기값으로 채워주기만 하면 수행이 가능하다.

`빌터 패턴`
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

- 가독성이 높은 코드를 작성할 수 있다.

자바 빈즈 같은 경우는 생성한 객체에 setter로 값을 모두 세팅을 해주어야 한다.

`자비 빈즈 패턴`
```java
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class SuccessCode {
    private int status;
    private String code;
    private String message;
}

SuccessCode successCode = new SuccessCode();
successCode.setStatus(200);
successCode.setCode("200");
successCode.setMessage("성공");
```
 빌더 패턴의 경우 하나의 메서드 묶음에 모두 값을 세팅할 수 있기에 가독성이 좋다.

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

// Builder Pattern 
SuccessCode successCode = SuccessCode.builder()
                .status(0)
                .message("성공 되었습니다.")
                .code("200")
                .build();
 ```

 - 변경 가능성을 최소화할 수 있다.

 Lombok에서 모든 변수의 생성자를 만들어주는 @AllArgsConstructor 어노테이션을 사용하는 경우에 IDE에서 제공하는 리펙토링이 작동하지 않고 개발자도 인식하지 못하는 사이에 생성자의 파라미터 순서를 필드 선언 순서에 맞춰 변경해버리는 문제가 있다.
 또한 인스턴스 멤버의 선언 순서에 영향을 받기 때문에 변수의 순서를 바꾸면 생성자의 입력 값 순서도 바뀌게 되어 검출되지 않는 치명적인 오류를 발생시킬 수 있다.

 ```java
 import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@AllArgsConstructor
public class SuccessCode {
    // 성공 코드의 '코드 상태'를 반환한다.
    private final int status;

    // 성공 코드의 '코드 값'을 반환한다.
    private final String code;

    // 성공 코드의 '코드 메시지'를 반환한다.
    private final String message;
}


/**
 * 문제점이 발생하는 구간
 * 생성자의 순서가 뒤바뀌더라도 객체 생성이 완료되어 잘못된 데이터가 들어 갈 수 있다.
 */

// 객체 생성 - OK
SuccessCode successCode = new SuccessCode(200, "200", "성공입니다.");
// 객체 생성 - OK
SuccessCode successCode = new SuccessCode(200, "성공입니다.", "200");
 ```

 - 선택적으로 객체를 구성할 수 있다.

 모든 인자를 넣어줘야 하는 생성자 형태에서 파라미터 값을 넣어주지 않으면 오류가 발생한다.

 builder로 생성자를 구성할 때에 값을 넣지 않으면 null로 값을 반환해주어서 원하는 선택적으로 생성자를 구성할 수 있다.