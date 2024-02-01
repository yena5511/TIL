## Constructor Binding

Spring 프레임워크로 개발을 하다 보면 프로퍼티(Properties)에 저장된 특정한 설정 값들을 불러와야 하는 경우가 있다. final 변수가 붙은 클래스에 설정 값을 불러오는 방법이다.

#### 생성자 바인딩(Constructor Binding)로 프로퍼티(Properties) 설정 값 불러오기

변경 가능성을 닫기 위해서는 해당 변수들을 모두 final로 선언하고, 생성자를 추가해주면 된다. 
그리고 생성자를 이용해 properties의 값을 바인딩하도록 @ConstructorBinding 어노테이션을 추가해주면 된다.

```java
@Getter
@RequiredArgsConstructor
@ConfigurationProperties(prefix = "spring.datasource")
@ConstructorBinding
public class DataSourceProperties {

	private final String driverClassName;
	private final String url;
	private final String username;
	private final String password;

}

```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdeJHt%2FbtreKvx22KL%2Ff4bfOLoQa2ZBSHaSbGceaK%2Fimg.png)

애플리케이션의 규모가 큰데, 변경 가능성이 열려 있어 변경 가능한 포인트가 많다면 나중에 유지보수가 어려워진다.
그러므로 불변 객체를 최대한 이용하도록 하고, 프로퍼티를 불러오는 경우에도 적용하도록 하자.
참고로 스프링부트 3.0부터는 1개의 생성자만 있는 경우에 기본적으로 생성자 방식으로 동작하도록 수정되었다고 한다. 
따라서 생성자가 2개 이상 존재하지 않는 경우라면 @ConstructorBinding를 생략해주어도 된다.

