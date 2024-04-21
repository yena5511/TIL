## Bridge Pattern

큰 클래스 또는 밀접하게 관련된 클래스들의 집합을 두 개의 개별 계층구조​(추상화 및 구현)​로 나눈 후 각각 독립적으로 개발할 수 있도록 하는 구조 디자인 패턴이다.

#### 구조

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNV0yM%2FbtrBUUwW5bS%2FcCPQxRUTlxKGCo1tdZkQt1%2Fimg.png)

- Abstraction  
    - 기능 계층의 최상위 클래스
    - 구현 부분에 해당하는 클래스를 인스턴스를 가지고 해당 인스턴스를 통해 구현부분의 메서드를 호출한다.
- RefindAbstraction 
    - 기능 계층에서 새로운 부분을 확장한 클래스
- Implementor 
    - Abstraction의 기능을 구현하기 위한 인터페이스 정의
- ConcreteImplementor
    -  실제 기능을 구현

#### 장점

- 클래스 계청을 분리할 때 완전한 인터페이스를 결합하지 않는다. 
이를 통해 독립적인 확장이 가능하다.
- 사용하면 런타임 시점에 어떤 방식으로 기능을 구현할지 선택할 수 있다.
- 상세한 기능을 외부로부터 숨길 수 있는 은닉 효과도 얻을 수 있다.

#### 단점

- 추상화를 통해 코드를 분리할 경우 코드 디자인 설계가 복잡해진다
