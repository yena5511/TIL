#### @NoArgsConstructor

기본생성자를 자동으로 추가한다

```
@NoArgsConstructor(access = AccessLevel.PROTECTED)

```

기본 생성자의 접근 권한을 protected로 제한한다
생성자로 protected Posts() {}와 같은 효과

Entity Class를 프로젝트 코드상에서 기본생성자로 생성하는 것은 금지하고, JPA에서 Entity 클래스를 생성하는것은 허용하기 위해 추가한다.

#### @AllArgsConstructor

모든 필드 값을 파라미터로 받는 생성자를 추가한다.

#### @Getter

Class 내 모든 필드의 Getter method를 자동으로 생성한다.

#### @Setter
Class 내 모든 필드의 Setter method를 자동 생성한다.

Controller에서 @RequestBody로 외부에서 데이터를 받는 경우엔 기본생성자 + set method를 통해서만 값이 할당된다.
그래서 이때만 setter를 허용한다.
Entity Class에는 Setter를 설정하면 안된다.
차라리 DTO 클래스를 생성해서 DTO 타입으로 받도록 하자


#### ToString

class 내 모든 필드의 toString method를 자동 생성한다.

`@ToString(exclude = "password")`
특정 필드를 toString() 결과에서 제외한다.
클래스명(필드1이름 = 필드 1값, 필드2 이름 = 필드2 값, ...)식으로 출력된다

#### @EqualsAndHadhCode

equals외 hashCode method를 오버라이딩 해주는 Annotation이다

`@EqualsAndHashCode(callSuper = true)`

callSuper 속성을 통해 equals와 hashCode 메소드 자동 생성 시 부모 클래스의 필드까지 감안할지 안 할지에 대해서 설정할 수 있다.
즉, callSuper = true로 설정하면 부모 클래스 필드 값들도 동일한지 체크하며, callSuper = false로 설정(기본값)하면 자신 클래스의 필드 값들만 고려한다.

#### @Builder

어느 필드에 어떤 값을 채워야 할지 명확하게 정하여 생성 시점에 값을 채워준다

#### Constructor와 Builder의 차이

생성 시점에 값을 채워주는 역할은 똑같다.
하지만 Builder를 사용하면 어느 필드에 어떤 값을 채워야 할지 명확하게 인지할 수 있다.
해당 Class의 Builder 패턴 Class를 생성 후 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함된다.

#### @Data

`@Getter @Setter @EqualsAndHashCode @AllArgsConstructor`을 포함한 Lombok에서 제공하는 필드와 관련된 모든 코드를 생성한다.

실제로는 사용하지 않는 것이 좋다
전체적인 모든 기능 허용으로 위험 존재