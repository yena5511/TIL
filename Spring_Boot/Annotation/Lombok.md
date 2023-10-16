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