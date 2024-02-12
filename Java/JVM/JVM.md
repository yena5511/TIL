## Java Virtual Machine(JVM)

Java는 OS에 종속적이지 않다는 특징을 가지고 있다.
OS에 종속적이지 않고 실행되기 위해선 OS 위에서 Java를 실행기킬 무언가가 필요한다.
그게 바로 JVM이다.
즉, OS에서 종속받지 않고 CPU가 Java를 인식, 실행할 수 있게하는 가상 컴퓨터이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0kg24%2Fbtq4YOOQH4J%2FEF2ISOpkYA36a1flwtLEmK%2Fimg.png)

Java는 JVM 이라는 가상머신을 거쳐서 OS에 도달하기 때문에 OS가 인식할 수 있는 기계어로 바로 컴파일 되는게 아니라 JVM이 인식할 수 있는 Java bytecode(*.class)로 변환된다.
Java compiler 가 .java 파일을 .class 라는 Java bytecode로 변환한다.

`💡 여기서 Java compiler는 JDK를 설치하면 bin 에 존재하는 javac.exe를 말한다. 
javac 명령어를 통해 .java를 .class로 컴파일 할 수 있다.`

변환된 bytecode는 기계어가 아니기 때문에 OS에서 바로 실행되지 않는다.
이 때, JVM이 OS가 bytecode를 이해할 수 있도록 해석해준다. 
따라서 Byte Code는 JVM 위에서 OS 상관없이 실행될 수 있는 것이다.


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

#### JVM 구조와 작동 원리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcssiwB%2FbtrDAQE2Zod%2FU7NTDqHKKGkKqpG3jOlBX0%2Fimg.png)

JVM의 구조에는 크게 Class Loader, Runtime data areas, Execution Engine으로 나누어져 있다.

**1. Class Loader**

JVM 내로 클래스 파일(*.class)을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다.
런 타임시 동적으로 클래스를 로드하고 jar 파일 내 저장된 클래스들을 JVM 위에 탑재한다.

**2. Execution Engine(실행 엔진)**

클래스를 실행 시키는 역할이다.
class파일과 같은 ByteCode를 실행 가능하도록 해석한다.

- 인터프리터:
실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다.
- JIT(Just_In_Time):
바이트 코드 전체를 컴파일 하여 기계어로 변경한다.
- GC(Garbage Collector):  메모리 관리 기법 중 하나로, Heap 영역에 배치된 객체들을 관리하는 모듈이다.
더 이상 사용하지 않는 인스턴스를 찾아 메모리에서 삭제한다.

**3. Runtime Data Area**

런타임 시 클래스 데이터와 같은 메타 데이터와 실제 데이터가 저장되는 곳이다.
간단하게 말하자면 프로그램을 수행하기 위해 OS로부터 할당받은 메모리 영역을 의미한다. (Java 메모리 공간)
Runtime Data Area에는 또다시 Method Area, Heap, PC Registers, Java Stacks, Native Method Stacks,  JVM 스택 영역으로 나누어진다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FXLtjO%2FbtrDyGDpp0C%2FK8wEGphqloy5uKZTC08Y7k%2Fimg.png)


- PC Register:
스레드가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로, 스레드마다 하나씩 존재한다.

- JVM 스택 영역:
프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 소멸되는 데이터를 저장하기 위한 영역이다.
메소드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성된다. 
메서드 수행이 끝나면 프레임 별로 삭제를 한다.

- Native method stack:
자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역이다.
자바가 아닌 다른 언어로 작성된 코드를 위한 공간.

- Method Area (= Class Area = Static area):
클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간
    - Runtime Constant Pool:
    상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.

- Heap 영역:
객체를 저장하는 가상메모리 공간. new 연산자로 생성되는 객체와 배열을 저장한다.

    - Permanent Generation:
    생성된 객체들의 정보의 주소값이 저장된 공간이다.
    Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.
    -  New/Young 영역:
    인스턴스들은 추후 가비지 콜렉터에 의해 사라진다.
    여기서 일어나는 가비지 콜렉트를 Minor GC 라고 한다.
    -  Old 영역:
    인스턴스들은 추후 가비지 콜렉터에 의해 사라진다.
    여기서 일어나는 가비지 콜렉트를 Major GC 라고 한다. Minor GC에 비해 속도가 느리다.
    New/Young Area에서 일정시간 참조되고 있는, 살아남은 객체들이 저장되는 공간이다.
