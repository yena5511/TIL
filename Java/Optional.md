## Optional

#### NPE이란

개발을 할 때 가장 많이 발생하는 예외 중 하나로 바로 NPE(NullPointException)이다.
NPE를 피하려면 null 여부를 검사해야 하는데, null 검사를 해야하는 변수가 많은 경우 코드가 복잡해지고 번거롭다.
그래서 null 대신 초기값을 사용하길 권장하기도 한다.

```java
List<String> names = getNames();
names.sort(); // names가 null이라면 NPE가 발생함

List<String> names = getNames();
// NPE를 방지하기 위해 null 검사를 해야함
if(names != null){
    names.sort();
}

```

#### Optional

Java8에서는 Optional<T> 클래스를 사용해 NPE를 방지할 수 있도록 도와준다.
Optional<T>는 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다.
Optional 클래스는 아래와 같은 value에 값을 저장하기 때문에 값이 null이더라도 바로 NPE가 발생하지 않으며, 클래스이기 때문에 각종 메소드를 제공해준다.

Optional은 null을 반환하면 오류가 발생할 가능성이 매우 높은 경우에 '결과 없음'을 명확하게 드러내기 위해 메소드의 반환 타입으로 사용되도록 매우 제한적인 경우로 설계되었다는 것이다. 
이러한 설계 목적에 부합하게 실제로 Optional을 잘못 사용하면 많은 부작용(Side Effect)이 발생하게 된다.

#### Optional 활용

Optional.empty() - 값이 Null인 경우

Optional은 Wrapper 클래스이기 때문에 값이 없을 수도 있는데, 이때는 Optional.empty()로 생성할 수 있다.

```java
Optional<String> optional = Optional.empty();

System.out.println(optional); // Optional.empty
System.out.println(optional.isPresent()); // false
```

Optional.of() - 값이 Null이 아닌 경우

만약 어떤 데이터가 절대 null이 아니라면 Optional.of()로 생성할 수 있다.
만약 Optional.of()로 Null을 저장하려고 하면 NullPointerException이 발생한다.

```java
// Optional의 value는 절대 null이 아니다.
Optional<String> optional = Optional.of("MyName");
```


Optional.ofNullbale() - 값이 Null일수도, 아닐수도 있는 경우

만약 어떤 데이터가 null이 올 수도 있고 아닐 수도 있는 경우에는 Optional.ofNullbale로 생성할 수 있다. 
그리고 이후에 orElse 또는 orElseGet 메소드를 이용해서 값이 없는 경우라도 안전하게 값을 가져올 수 있다.

```java
// Optional의 value는 값이 있을 수도 있고 null 일 수도 있다.
Optional<String> optional = Optional.ofNullable(getName());
String name = optional.orElse("anonymous"); // 값이 없다면 "anonymous" 를 리턴
```
####  올바른 Optional 사용법

- Optional, 변수에 Null울 할당하지 말아라
- 값이 없을 때 Optional.orElseX()로 기본 값을 반환하라
- 단순히 값을 얻으려는 목적으로만 Optional을 사용하지 마라
- 생성자, 수정자, 메소드 파라미터 등으로 Optional을 넘기지 마라
- Collection의 경우 Optional이 아닌 빈 Collection을 사용하라
- 반환 타입으로만 사용하라
