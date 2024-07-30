## Flyweight Pattern

재사용 가능한 객체 인스턴스를 공유시켜 메모리 사용량을 최소화하는 구조 패턴이다.
자주 변화는 속성(extrinsit)과 변하지 않는 속성(intrinsit)을 분리하고 변하지 않는 속성을 캐시하여 재사용해 메모리 사용을 줄이는 방식이다. 

#### Flyweight Pattern 구조

![](https://images.velog.io/images/secdoc/post/f9894bb1-2d31-4720-aa78-f32c19ddb1c7/uml.PNG)

- Flyweight : 경량 객체를 묶는 인터페이스
- ConcreteFlyweight : 공유 가능하여 재사용되는 객체 (intrinsic state)
- UnsahredConcreteFlyweight : 공유 불가능한 객체 (extrinsic state)
- FlyweightFactory : 경량 객체를 만드는 공장 역할과 캐시 역할을 겸비하는 Flyweight 객체 관리 클래스
    - GetFlyweight() 메서드는 팩토리 메서드 역할을 한다고 보면 된다
    - 만일 객체가 메모리에 존재하면 그대로 가져와 반환하고, 없다면 새로 생성해 반환한다
- Client : 클라이언트는 FlyweightFactory를 통해 Flyweight 타입의 객체를 얻어 사용한다.

#### 장점

- 프로그램에 유사한 객체들이 많다고 가정하면 많은 RAM을 절약
- 프로그램 속도를 개선
    - new로 인스턴스화를 하면 데이터가 생성되고 메모리에 적재 되는 미량의 시간
    - 객체를 공유하면 인스턴스를 가져오기만 하면 되기 때문에 메모리 뿐만 아니라 속도도 향상


#### 단점
- 코드의 복잡도가 증가