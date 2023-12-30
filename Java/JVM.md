## Java Virtual Machine(JVM)

가상 머신이란 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것

Java는 OS에 종속적이지 않다는 특징을 가지고 있다.
OS에 종속적이지 않고 실행되기 위해선 OS 위에서 Java를 실행기킬 무언가가 필요한다.
그게 바로 JVM이다.
즉, OS에서 종속받지 않고 CPU가 Java를 인식, 실행할 수 있게하는 가상 컴퓨터이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0kg24%2Fbtq4YOOQH4J%2FEF2ISOpkYA36a1flwtLEmK%2Fimg.png)

Java 소스코드, 즉 원시코드(*.java)는 CPU가 인식을 하지 못하므로 기계어로 컴파일을 해줘야한다.
하지만 Java는 이 JVM 이라는 가상머신을 거쳐서 OS에 도달하기 때문에 OS가 인식할 수 있는 기계어로 바로 컴파일 되는게 아니라 JVM이 인식할 수 있는 Java bytecode(*.class)로 변환된다.
Java compiler 가 .java 파일을 .class 라는 Java bytecode로 변환한다.

`💡 여기서 Java compiler는 JDK를 설치하면 bin 에 존재하는 javac.exe를 말한다. (즉, JDK에 Java compiler가 포함되어 있다는 소리임)
javac 명령어를 통해 .java를 .class로 컴파일 할 수 있다.`

변환된 bytecode는 기계어가 아니기 때문에 OS에서 바로 실행되지 않는다.
이 때, JVM이 OS가 bytecode를 이해할 수 있도록 해석해준다. 
따라서 Byte Code는 JVM 위에서 OS 상관없이 실행될 수 있는 것이다.
OS에 종속적이지 않고, Java 파일 하나만 만들면 어느 디바이스든 JVM 위에서 실행할 수 있다.

#### 컴파일 하는 방법

Java Compiler에 의해 .java 파일을 .class 라는 Java bytecode로 만드는 과정이다.
Java Compiler는 JDK를 설치하면 javac.exe라는 실행 파일 형태로 설치된다. 
정확히는 JDK 의 bin 폴더에 javac.exe 로 존재한다.
Java Complier 의 javac 라는 명령어를 사용하면 .class 파일을 생성할 수 있다.

```java
public class test {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

"Hello World"를 출력하는 .java 파일을 생성하고 이를 .class 파일로 변환시켜보자.

```cmd
C:\Users\owner>cd Desktop
```
Windows를 기준으로, cmd 창을 열고 해당 .java 파일이 있는 곳으로 이동한다.

```cmd
C:\Users\owner\Desktop>javac test.java
```
해당 위치에서 javac 명령어로 컴파일을 진행한다.

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
