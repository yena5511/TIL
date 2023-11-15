## 상속의 단점

- 캡슐화를 위반한다
    - 상위 클래스가 어떻게 구현되느냐에 따라 하위 클래스의 동작에 이상이 생길 수 있기 때문이다.
    - 상위 클래스의 내부 구현이 달라지면 하위 클래스를 고쳐야할 수 있다.

- 설계가 유연하지 못한다 
    - 컴파일 시점에 객체의 Type이 정해지기 떄문이다.

- 다중상속
    - 자바는 다중상속이 불가능하다.
    - 따라서 다른 클래스를 상속받고 있다면 추가적으로 상속을 받을 수 없다.

## 컴포지션

- 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해서 메서드를 호출하는 기법이다.
- 해당 인스턴스의 내부 구현이 바뀌더라도 영향을 받지 않는다.
- 또한, 다른 객체의 인스턴스이므로 인터페이스를 이용하면 Type을 바꿀 수 있다.
- 상속처럼 기존의 클래스를 확장(extend)하는 것이 아닌, 새로운 클래스를 생성하여 private 필드로 기존 클래스의 인스턴스를 참조하는 방식이 바로 컴포지션이다.

`예시`

```java
public class CompositionExample {

    public static void main(String... houseComposition) {
        new House(new Bedroom(), new LivingRoom());
        // The house now is composed with a Bedroom and a LivingRoom
    }

    static class House {

        Bedroom bedroom;
        LivingRoom livingRoom;

        House(Bedroom bedroom, LivingRoom livingRoom) {
            this.bedroom = bedroom;
            this.livingRoom = livingRoom;
        }
    }
    static class Bedroom { }
    static class LivingRoom { }
}
```

- house에는 침실과 거실이 있기 때문에 bedroom과 livingroom object를 house의 컴포지션으로 사용이 가능하다.
    - A house has a living room (a living room is part of a house).

## 예제로 확인하는 상속과 컴포지션 사용

#### 상속
```java
import java.util.HashSet;

public class CharacterBadExampleInheritance extends HashSet<Object> {

    public static void main(String... badExampleOfInheritance) {
        BadExampleInheritance badExampleInheritance = new BadExampleInheritance();
        badExampleInheritance.add("Homer");
        badExampleInheritance.forEach(System.out::println);
    }
```
- 매우 적합하지 않다
    - HashSet을 상속받아놓고 BadExampleInheritance에 대한 인스턴스를 생성 후 사용한다.
    - 즉 하위 클래스인 CharacterBadExampleInheritance는 사용하지 않은 많은 메서드를 상속받으므로 혼란스럽고 유지하기 어려운 tightly coupled code 즉 결합도가 높은 코드가 생성된다.

#### 컴포지션


```java
import java.util.HashSet;
import java.util.Set;

public class CharacterCompositionExample {
    static Set<String> set = new HashSet<>();

    public static void main(String... goodExampleOfComposition) {
        set.add("Homer");
        set.forEach(System.out::println);
```
- 위 코드 시나리오에서 컴포지션을 사용하면 CharacterCompositionExample는 HashSet을 상속하지 않고 HashSet의 메서드 중 단지 두 가지만 사용하게 된다.
- 그 결과 단순하고 이해하기 쉽고 유지하기 쉬운 less coupled code 즉 결합도가 낮은 코드가 생성된다.