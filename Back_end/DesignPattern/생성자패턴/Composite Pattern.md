## Composite Pattern

Composite는 
컴포지션 패턴은 트리구조를 작성하고 싶은 때 사용하며, 전체 - 부분 관계를 표현하는 것이다.
하나의 객체를 호출하면 서브로 갖고 있는 자식의 객체 메서드를 호출할 수도 있다.


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbllXnv%2FbtrvcHDNz4J%2FN5uyJro0ITwOWbVkJPNcXK%2Fimg.png)

- Component
    - 컴포넌트 역할을 수행하는 추상 클래스
    - 노드인 Composite와 마지막 노드인 Leaf에 공통으로 적용된다.
    - Composite와 leaf는 동일한 처리를 위해 Component 추상 클래스를 상속받는다.
- Composite
    - 복합체 패턴은 다른 복합체 패턴을 포함할 수도 있고 마지막 노드가 될 수도 있다.
- Leaf
    - 복합체는 계층적 트리 구조로 되어있다.
    - 노드의 제일 마지막 객체이다.
- Client
    - 복합체를 호출하는 클래스

#### 장점

- 객체에 따라 다르게 처리하지 않고 동일하게 취급해야 할 때 사용
- 투명성을 이용해 클라이언트 사용을 단순화할 수 있음

#### 단점

- 재귀 호출의 특징 상 트리의 Depth가 길어질수록 라인 단위의 디버깅에 어려움이 생김
- 복합체 패턴은 수평적, 수직적 모든 방향으로 객체를 확장할 수 있으나 수평적 방향으로만 확장하도록 Leaf를 제한한 Composite를 만들기는 어려움