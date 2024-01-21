## Telescoping Constructor Pattern(점층적 생성자 패턴)

생성자를 매개변수에 개수만큼 구성하는 패턴을 의미한다.
점층적 생성자 패턴은 필요할 수 있는 모든 매개변수 조합의 생성자를 작성하는 방법이다.
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

    // 모든 매개변수를 가짐
    SuccessCode(int status, String code, String message) {
        this.status = status;
        this.code = code;
        this.message = message;
    }
}
```

객체를 생성하고자 할 때 객체 내에 파리키터 값을 필수적으로 넣어줘야 구성이 가능하다.

#### 장점

객체가 불완전한 상태에 있는 것을 방지할 수 있다.

#### 단점

클라이언트 코드의 가독성이 떨어진다.
매개변수가 많아지면 많아질 수록, 코드를 읽을 때 각 매개변수의 순서와 개수를 주의해서 보아야 그 의미를 파악할 수 있다. 
또, 타입이 같은 매개변수가 연달아 있을 경우, 컴파일 단계에서도 알아차릴 수 없는 버그가 발생할 수 있다.
