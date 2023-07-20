## 메소드

- 반복문, 조건문, 변수, 상수와 같은 것들은 프로그램을 만드는 가장 중요한 도구들이라고 할 수 있다.
- 메소드나 객체지향과 같은 개념들은 애플리케이션을 만들기 위한 기법들이라고 할 수 있다.

##### 메소드
 메소드는 코드를 재사용 할 수 있게 해준다. 

 #### 메소드 형식
 ```java
 public static void main(String[] args){
    return
 }
 ```

#### 메소드의 정의(define)와 호출(call)
![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1835.gif)
#### main

- 자바와 개발자 사이의 약속
- main 메소드를 작성하고, 자바는 main 메소드를 실행하는 관계라고 할 수 있다.


#### 메소드가 없다면

```java
package org.opentutorials.javatutorials.method;

public class MethodDemo2 {
     
    public static void main(String[] args) {
        int i = 0;
        while(i<10){
            System.out.println(i);
            i++;
        }
         
        i = 0;
        while(i<10){
            System.out.println(i);
            i++;
        }
         
        i = 0;
        while(i<10){
            System.out.println(i);
            i++;
        }
         
        i = 0;
        while(i<10){
            System.out.println(i);
            i++;
        }
         
        i = 0;
        while(i<10){
            System.out.println(i);
            i++;
        }
    }
 
}
```
*메소드 사용*
```java
package org.opentutorials.javatutorials.method;
 
public class MethodDemo3 {
    public static void numbering() {
        int i = 0;
        while (i < 10) {
            System.out.println(i);
            i++;
        }
    }
 
    public static void main(String[] args) {
        numbering();
        numbering();
        numbering();
        numbering();
        numbering();
    }
}
```
- 코드의 량을 줄일 수 있다.
- 유지보수가 유리

#### 입력과 출력

- 외부의 자극이 입력이면 반응은 출력이라고 할 수 있다.
- 메소드는 항상 똑같은 동작만 반복한다.

#### 매개변수와 인자

![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1838.gif)
- int limit = 매개변수
- numbering **(5)** = 인자

#### return 

메소드 밖으로 돌려준다.

```java
package org.opentutorials.javatutorials.method;
 
public class MethodDemo6 {
    public static String numbering(int init, int limit) {
        int i = init;
        // 만들어지는 숫자들을 output이라는 변수에 담기 위해서 변수에 빈 값을 주었다.
        String output = "";
        while (i < limit) {
            // 숫자를 화면에 출력하는 대신 변수 output에 담았다.
            output += i;
            i++;
        }
        // 중요!!! output에 담겨 있는 문자열을 메소드 외부로 반환하려면 아래와 같이 return 키워드 뒤에 반환하려는 값을
        // 배치하면 된다.
        return output;
    }
 
    public static void main(String[] args) {
        // 메소드 numbering이 리턴한 값이 변수 result에 담긴다.
        String result = numbering(1, 5);
        // 변수 result의 값을 화면에 출력한다.
        System.out.println(result);
    }
}
```

- 복잡하게 데이터를 리턴하는 이유는 무엇일까?
    - 부품으로서의 가치를 높이기 위해서


```java
package org.opentutorials.javatutorials.method;
 
import java.io.*; // 무시
 
public class MethodDemo7 {
    public static String numbering(int init, int limit) {
        int i = init;
        String output = "";
        while (i < limit) {
            output += i;
            i++;
        }
        return output;
    }
 
    public static void main(String[] args) {
        String result = numbering(1, 5);
        System.out.println(result);
        try { // 무시
            // 다음 행은 out.txt 라는 파일에 numbering이라는 메소드가 반환한 값을 저장합니다.
            BufferedWriter out = new BufferedWriter(new FileWriter("out.txt"));
            out.write(result);
            out.close();
        } catch (IOException e) {
        } // 무시
    }
}
```

#### 복수의 리턴

- 메소드는 여러 개의 입력 값을 가질 수 있다.

```java
package org.opentutorials.javatutorials.method;
 
public class ReturnDemo4 {
 
    public static String[] getMembers() {
        String[] members = { "최진혁", "최유빈", "한이람" };
        return members;
    }
 
    public static void main(String[] args) {
        String[] members = getMembers();
    }
 
}
```


## 객체지향 프로그래밍(Object-Oriented Programming)<br>
● 객체지향 프로그래밍은 좀 더 나은 프로그램을 만들기 위한 프로그래밍 패러다임으로 로직을 상태(state)와 행위(behave)로 이루어진 객체로 만드는 것이다. 
#### 객체<br>
: 클래스의 인스턴스(Instance)로 자신 고유의 데이터를 가지며 클래스에서 정의한 행위를 수행할 수 있다.


## 클래스와 인스턴스 그리고 객체

#### 은닉화, 캡슐화 
: 내부의 동작 방법을 단단한 케이스(객체) 안으로 숨기고 사용자에게는 그 부품의 사용방법(메소드)만을 노출하고 있는 것이다.

#### 클래스
:연관되어 있는 변수와 메소드의 집합이다.
- 필요한 이유 
    - 프로그래밍의 기본은 중복을 제거한다


 #### 인스턴스
 ■ 문법 형태 : Calculator c1 = new Calculator();


```
public class Main {
    public static void main(String[] args) {
Calculator c1 = new Calculator();
c1.setOprands(10, 20);
c1.sum();       
c1.avg();       
 
Calculator c2 = new Calculator();
c2.setOprands(20, 40);
c2.sum();       
c2.avg();
    }
}
```

## 클래스 맴버와 인스턴스 맴버


#### 맴버
- 맴버(member)는 영어로 구성원이라는 뜻이다.
    - 변수
    - 메소드



#### 클래스 변수

인스턴스 변수는 인스턴스마다 다른 값을 가지게 되는 변수인데 클래스 변수는 클래스의 변수이기 때문에 그 클래스에 따라 만들어진 모든 인스턴스는 그 클래스가 가지고 있는 값을 자연스럽게 가진다.


```
package org.opentutorials.javatutorials.classninstance;
 
class Calculator {
    static double PI = 3.14;
    int left, right;
 
    public void setOprands(int left, int right) {
        this.left = left;
        this.right = right;
    }
 
    public void sum() {
        System.out.println(this.left + this.right);
    }
 
    public void avg() {
        System.out.println((this.left + this.right) / 2);
    }
}
 
public class CalculatorDemo1 {
 
    public static void main(String[] args) {
 
        Calculator c1 = new Calculator();
        System.out.println(c1.PI);
 
        Calculator c2 = new Calculator();
        System.out.println(c2.PI);
 
        System.out.println(Calculator.PI);
 
    }
 
}


```

- 클래스 변수의 용도를 정리해보면 아래와 같다.

    - 인스턴스에 따라서 변하지 않는 값이 필요한 경우 (위의 예에서는 PI) (이런 경우 final을 이용해서 상수로 선언하는 것이 바람직 하지만 final을 아직 배우지 않았기 때문에 언급하지 않았다)
    - 인스턴스를 생성할 필요가 없는 값을 클래스에 저장하고 싶은 경우
    - 값의 변경 사항을 모든 인스턴스가 공유해야 하는 경우



#### 클래스 메소드


클래스 메소드를 사용하는 이유 : 
메소드가 인스턴스 변수를 참조하지 않는다면 클래스 메소드를 사용해서 불필요한 인스턴스의 생성을 막을 수 있다.


```
package org.opentutorials.javatutorials.classninstance;
 
class Calculator3{
  
    public static void sum(int left, int right){
        System.out.println(left+right);
    }
     
    public static void avg(int left, int right){
        System.out.println((left+right)/2);
    }
}
 
public class CalculatorDemo3 {
     
    public static void main(String[] args) {
        Calculator3.sum(10, 20);
        Calculator3.avg(10, 20);
         
        Calculator3.sum(20, 40);
        Calculator3.avg(20, 40);
    }
 
}
```


#### 맴버타입의 비교


1. 인스턴스 메소드는 클래스 맴버에 접근 할 수 있다.
2. 클래스 메소드는 인스턴스 맴버에 접근 할 수 없다.
 - 클래스는 메소드 보다 먼저 존재
 -인스턴스 변수는 인스턴스가 만들어지면서 생성되는데, 클래스 메소드는 인스턴스가 생성되기 전에 만들어지기 때문에 클래스 메소드가 인스턴스 맴버에 접근하는 것은 존재하지 않는 인스턴스 변수에 접근하는 것과 같다.
 
 예제
 ```
 package org.opentutorials.javatutorials.classninstance;
 
class C1{
    static int static_variable = 1;
    int instance_variable = 2;
    static void static_static(){
        System.out.println(static_variable);
    }
    static void static_instance(){
        // 클래스 메소드에서는 인스턴스 변수에 접근 할 수 없다. 
        //System.out.println(instance_variable);
    }
    void instance_static(){
        // 인스턴스 메소드에서는 클래스 변수에 접근 할 수 있다.
        System.out.println(static_variable);
    }
    void instance_instance(){        
        System.out.println(instance_variable);
    }
}
public class ClassMemberDemo {  
    public static void main(String[] args) {
        C1 c = new C1();
        // 인스턴스를 이용해서 정적 메소드에 접근 -> 성공
        // 인스턴스 메소드가 정적 변수에 접근 -> 성공
        c.static_static();
        // 인스턴스를 이용해서 정적 메소드에 접근 -> 성공
        // 정적 메소드가 인스턴스 변수에 접근 -> 실패
        c.static_instance();
        // 인스턴스를 이용해서 인스턴스 메소드에 접근 -> 성공
        // 인스턴스 메소드가 클래스 변수에 접근 -> 성공
        c.instance_static();
        // 인스턴스를 이용해서 인스턴스 메소드에 접근 -> 성공 
        // 인스턴스 메소드가 인스턴스 변수에 접근 -> 성공
        c.instance_instance();
        // 클래스를 이용해서 클래스 메소드에 접근 -> 성공
        // 클래스 메소드가 클래스 변수에 접근 -> 성공
        C1.static_static();
        // 클래스를 이용해서 클래스 메소드에 접근 -> 성공
        // 클래스 메소드가 인스턴스 변수에 접근 -> 실패
        C1.static_instance();
        // 클래스를 이용해서 인스턴스 메소드에 접근 -> 실패
        //C1.instance_static();
        // 클래스를 이용해서 인스턴스 메소드에 접근 -> 실패
        //C1.instance_instance();
    }
 
}
```

`용어`<br>

인스턴스 변수 -> Non-Static Field<br>
클래스 변수 -> Static Field<br>
필드(field)라는 것은 클래스 전역에서 접근 할 수 있는 변수를 의미



## 유효범위


#### 유효범위란?

변수와 메소드 같은 것들을 사용할 수 있는 것은 이름이 있기 때문이다. 아래 코드에서 left는 변수의 이름이고, sum은 메소드의 이름이다.

```
int left;blic void sum(){}
```

프로그램이 커지면 여러 가지 이유로 이름이 `충돌`하게 된다. 이를 해결하기 위해서 고안된 것이 유효범위라는 개념이다. 흔히 `스코프(Scope)`라고도 부른다.

#### 출현 범위

- 유효범위는 메소드나 클래스처럼 특별한 문법적인 규칙을 가지고 있는 것은 아니다.
- 메소드나 클래스 안에 포함되어서 이러한 기능들의 부품으로서의 가치를 높여주는 역할을 한다고 할 수 있다.


#### 다양한 유효범위들

- 유효범위란 변수를 전역변수, 지역변수 나눠서 좀 더 관리하기 편리하도록 한 것이다. 
- 이름의 충돌이 이라는 현상을 해결하는 방법 디렉터리라고 할 수 있다.

- 클래스 아래에서 선언된 변수는 클래스 전역에 영향을 미치지만 메소드 내에서 선언된 변수는 클래스 아래에서 선언된 변수보다 우선순위가 높다고 할 수 있다. 
- 클래스 전역에서 접근 할 수 있는 변수를 전역변수, 메소드 내에서만 접근 할 수 있는 변수를 지역변수라고 한다.          
```
package org.opentutorials.javatutorials.scope;
 
public class ScopeDemo4 {
    static void a(){
        String title = "coding everybody";
    }
    public static void main(String[] args) {
        a();
        //System.out.println(title); (주석을 제거하면 오류가 발생할 것이다. title은 메소드 a에서만 유효하기 때문이다.) 
    }
 
}
```   


## 초기화와 생성자


#### 초기화
- 객체지향 프로그래밍도 초기화에 해당하는 기능이 제공되는데 이것을 생성자라고 한다.


#### 생성자

- 생성자는 그 이름처럼 객체에서 생성할 때 호출된다. 
- 생성자의 특징
    - *값을 반환하지 않는다.*<br>
    생성자는 인스턴스를 생성해주는 역할을 하는 특수한 메소드라고 할 수 있다. 그런데 반환값이 있다면 엉뚱한 객체가 생성될 것이다. 따라서 반환 값을 필요로하는 작업에서는 생성자를 사용하지 않는다. 반환 값이 없기 때문에 return도 사용하지 않고, 반환 값을 메소드 정의에 포함시키지도 않는다.
    - *생성자의 이름은 클래스의 이름과 동일하다* <br>
    자바에서 클래스의 이름과 동일한 메소드느 생성자로 사용하기로 약속되어 있다.


## 상속

#### 상속의 개념

- 어떤 객체가 있을 때 그  객체의 필드(변수)와 메소드를 다른 객체가 물려 받을 수 있는 기능을 상속이라고 한다.











                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    