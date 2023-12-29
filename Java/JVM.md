## Java Virtual Machine(JVM)

가상 머신이란 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것

JVM의 역할은 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것이다.
그리고 JVM은 Java와 OS(운영체제)에 구해받지 않고 독립적으로 작동이 가능하다.
또한 가장 중요한 메모리 관리, Garbage collection(가비지 컬렉션)을 수행한다.

#### JVM 특징

- 컴파일된 바이트 코드를 기계가 이해할 수 있는 기계어로 변환
- 스택 기반의 가상 머신
- 메모리 관리와 GC를 수행

#### JVM 구조와 작동 원리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcssiwB%2FbtrDAQE2Zod%2FU7NTDqHKKGkKqpG3jOlBX0%2Fimg.png)

JVM의 구조에는 크게 Class Loader, Runtime data areas, Execution Engine, GC 으로 나누어져 있다.

- Class Loader: 클래스 파일을 Runtime Data Area의 메서드 영역으로 불러오는 역할을 한다.
- Execution Engine: class파일과 같은 ByteCode를 실행 가능하도록 해석한다.
- GC(Garbage Collector):  메모리 관리 기법 중 하나로, Heap 영역에 배치된 객체들을 관리하는 모듈이다.
- Runtime Data Area: 런타임 시 클래스 데이터와 같은 메타 데이터와 실제 데이터가 저장되는 곳이다.
간단하게 말하자면 프로그램을 수행하기 위해 OS로부터 할당받은 메모리 영역을 의미한다. (Java 메모리 공간)
Runtime Data Area에는 또다시 Method Area, Heap, PC Registers, Java Stacks, Native Method Stacks로 나누어진다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FXLtjO%2FbtrDyGDpp0C%2FK8wEGphqloy5uKZTC08Y7k%2Fimg.png)

Java는 멀티 스레드 환경으로 모든 스레드는 Heap, Method Area를 공유한다.
