## Garbage Collection

프로그램을 개발 하다 보면 유효하지 않은 메모리인 가바지(Garbage)가 발생하게 된다.
C언어를 이용하면 free()라는 함수를 통해 직접 메모리를 해제해주어야 한다. 
하지만 Java나 Kotlin을 이용해 개발을 하다 보면 개발자가 메모리를 직접 해제해주는 일이 없다. 
그 이유는 JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리해주기 때문이다. 
대신 Java에서 명시적으로 불필요한 데이터를 표현하기 위해서 일반적으로 null을 선언해준다.

ex)

```java
Product product = new Product("TV");

sellTo(product);

product = null; //Product("TV") 사용되지 않음
없어짐
```
위 코드를 보면, new Product("TV") 가 메모리 어딘가에 할당되었을 것이다. 
그리고 해당 메모리 영역을 가르키던 "product"가 null이 되었다.

#### 장점

- 이미 해제된 메모리에 접근하는 버그를 방지해준다.
- 이미 해제된 메모리를 또다시 해제하는 버그를 방지해준다.
- 메모리 누수를 방지한다.

#### 단점

- 메모리가 해제되는 시점을 알 수가 없다.
- 해제할 메모리를 결정하는데 비용이 든다.
- 쓰레기 수집이 이루어지는 시점을 예측할 수가 없어서, 어떤 타이밍에 점유당할지 모른다.

#### Minor GC/ Major GC

JVM의 Heap 영역은 처음 설계될 때 다음 2가지를 전제로 설계 되었다.

- 대부분의 객체는 금방 접근 불가능한 상태가 된다.
- 오래된 객체에서 새로운 객체로 참조는 아주 적게 존재한다.

즉, 객체는 대부분 일회성이 되며, 메모리에 오랫동안 남아있는 경유는 드물다는 것이다.
그렇게 때문에 객체의 생존 기간에 따라 물리적인 Heap 영역을 나누게 되었고 Young, Old 총 2가지 영역으로 설계되었다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fva8qQ%2FbtqUSpSocbS%2FkxTvtnmrdhf4bnVPXth0UK%2Fimg.png)

- Young 영역
    - 새롭게 생성된 객체가 할당 되는 여역
    - 대부분의 객체가 금장 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
    - Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.

- Old 영역
    - Young영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
    - Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가바지는 적게 발생한다.
    - Old 영역에 대한 가비지 컬렉션을 Major GC라고 부른다.

 
Old 영역이 Young 영역보다 크게 할당되는 이유는 Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않으며 큰 객체들은 Young 영역이 아니라 바로 Old 영역에 할당되기 때문이다.
예외적인 상황으로 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우도 존재할 것이다.
이러한 경우를 대비하여 Old 영역에는 512 bytes의 덩어리(Chunk)로 되어 있는 카드 테이블(Card Table)이 존재한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFOLU3%2FbtqUOBF35cJ%2FBMKuD1iqfq6R0lAqMlfkC0%2Fimg.png)

카드 테이블에는 Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때 마다 그에 대한 정보가 표시된다. 
카드 테이블이 도입된 이유는 간단한다. 
Young 영역에서 가비지 컬렉션(Minor GC)가 실행될 때 모든 Old 영역에 존재하는 객체를 검사하여 참조되지 않는 Young 영역의 객체를 식별하는 것이 비효율적이기 때문이다. 
그렇기 때문에 Young 영역에서 가비지 컬렉션이 진행될 때 카드 테이블만 조회하여 GC의 대상인지 식별할 수 있도록 하고 있다.
