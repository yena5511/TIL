## Lombok

- 자바 라이브러리로 반복되는 getter, setter, toString 등의 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리이다.
이는 코드의 가독성을 향상기키고 개발 생산성을 높일 수 있다.
- 어노테이션을 사용하여 코드를 간결하게 작성하고, 컴파일 시점에서 핗요한 코드를 자동으로 생성한다.
따라서 Lombok을 사용하면 개발자는 더 적은 노력으로 효율적인 코드를 작성할 수 있다.

#### Lombok의 장점

- 어노테이션 기반의 코드 자동 생성을 통한 생산성 향상
- 반복되는 코드 다이어트를 통한 가독성 및 유지보수성 향상
- Getter, Setter 외에 빌더 패턴이나 로그 생성 등 다양한 방면으로 활용 가능


#### Lombok Annotation

|Annotation|설명|
|:---|:---|
|@Geetter|클래스의 모든 필드에 대한 getter 메서드를 생성한다.|
|@Setter|클래스의 모든 필드에 대한 setter 메서드를 생성한다.|
|@ToString|클래스의 toString()메서드를 생성한다.|
|@EqualsAndHashCode|클래스에 대한 equals() 및 hashCode() 메서드를 생성한다.|
|@NoArgsConstructor|클래스에 대한 인자가 없는 생성자를 생성한다.|
|@AllArgsConstructor|클래스의 각 필드에 대해 하나의 매개변수가 있는 생성자를 생성한다.|
|@RequiredArgsConstructor|클래스의 각 final 필드와 null이 아닌 필드에 대해 하나의 매개변수가 있는 생성자를 생성한다|
|@Data|	클래스의 모든 필드에 대한 getter 및 setter 메서드, toString() 및 equals() 및 hashCode() 메서드를 생성한다.|
|@Builder|	클래스에 대한 빌더 패턴을 생성한다.|
|@Log|내장 Java 로깅 프레임워크를 사용하여 클래스에 대한 로거 인스턴스를 생성한다.|
|@Log4j|Log4j 로깅 프레임워크를 사용하여 클래스에 대한 로거 인스턴스를 생성한다.|

