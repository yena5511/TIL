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

## 입력과 출력

#### String[] args

```java
package org.opentutorials.javatutorials.io;
 
class InputDemo{
    public static void main(String[] args){
        System.out.println(args.length);
    }
}
```

- String[] = 문자열을 답을 수 있는 배열

- void = 출력값 x

- String[] args =  메소드가 호출될 때 전달된 입력 값을 메소드 내부로 전달하는 역할을 하는 변수 

- args.length = 배열의 길이 


```java
class InputForeachDemo{
    public static void main(String[] args){
        for(String e : args){
            System.out.println(e);
        }
    }
}
```
- 메소드 main의 인자 String[] args를 통해서 사용자가 입력한 값을 전달하고 있다.


#### 앱이 실행중에 입력 받기

- 자바에서 기본적으로 제공하는 라이브러리 중에 scanner을 이용하면 쉽게 사용자의 입력을 잡을 수 있다. 

```java
package org.opentutorials.javatutorials.io;
 
import java.util.Scanner;
 
public class ScannerDemo {
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int i = sc.nextInt();
        System.out.println(i*1000);
        sc.close();
    }
 
}
```

- import java.util.Scanner = 로직으로 사용

- System,in = 사용자가 입력한 값


```java
package org.opentutorials.javatutorials.io;
 
import java.util.Scanner;
 
public class Scanner2Demo {
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNextInt()) {
            System.out.println(sc.nextInt()*1000); 
        }
        sc.close();
    }
 
}
```
- sc.hasNextInt()는 입력값이 생기기 전까지 실행을 유보시키는 역할을 한다.<br> 
만약 입력한 값이 int 형이 아닐 경우는 false를 리턴하고, int로 표현할 수 있는 형식의 숫자형인 경우는 true를 리턴한다.<br> 
따라서 위의 코드는 사용자가 입력을 할 때가지 실행을 기다렸다가 입력이 일어나면 반복문이 동작하면서 입력값의 1000배를 곱한 수가 출력된다. 


## 객체 지향 프로그래밍(Object-Oriented Programming)<br>

- 객체: 프로그램을 구성하는 로직을 상태와 행위로 구분해서 서로 연관되어 있는 상태나 행위호 그룹핑 한것<br>
= 변수와 메소드를 그룹핑한 것


#### 설계
- 프로그래밍: 현실에서 관심있는 어떠한 특성또는 어떤 관점을 sw화 시켜서 해결하는 것

![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1854.gif)
- 좌측 상단: 현실을 가장 충실히 반영하고 있는 지도
- 좌측 하단: 현실의 복잡한 것을 걷어내고 지하철의 노선을 그리고 있는 지도 
- 우측 하단: 지하철 탑승자의 관심사만을 반영

- 추상화: 복잡함 속에서 필요한 관점만을 추출하는 행위

#### 부품화

![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1857.gif)

- 하나의 부품이면서 완제품이다.<br>
하나가 고장 나면 컴퓨터를 바꿔야 한다

![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1856.gif)
- 모니터, 본체, 키보드, 마우스로 부픔화
- 문제가 생겼을 때 그 문제가 어디에서 발생한 것인지 파악하고 해결하기가 훨씬 쉬워진다

**부품화의 특성**
- 메소드는 부품화의 예라고 할 수 있다. 
- 메소드를 사용하면 코드의 양을 극적으로 줄일 수 있고, 메소드 별로 기능이 분류되어 있기 때문에 필요한 코드를 찾기도 쉽고 문제의 진단도 빨라진다.

#### 은닉화, 캡슐화

- 내부의 동작 방법을 단단한 케이스(객체) 안으로 숨기고 사용자에게 그 부품의 사용방법(메소드)만을 노출 하고 있는 것<br>
= 정보의 은닉화<br>
= 캡슐화

#### 인터페이스
- 잘 만들어진 부품이라면 부품과 부품을 서로 교환 할 수 있어야 한다
- 인터페이스: 장치와 장치를 연결하는 연결점


## 클래스와 인스턴스 그리고 객체

##### 클래스와 인스턴스 이전의 프로그래밍

- 클래스 = 객체를 만들기 위한 설계도
- 인스턴스 = 구체적인 제품

**중복되는 코드**
```java
package org.opentutorials.javatutorials.object;
 
public class CalculatorDemo {
 
    public static void main(String[] args) {
        // 아래의 로직이 1000줄 짜리의 복잡한 로직이라고 가정하자.
        System.out.println(10 + 20);
        System.out.println(20 + 40);
    }
 
}
```
#### 메소드화

```java
package org.opentutorials.javatutorials.object;
 
public class CalculatorDemo2 {
 
    public static void sum(int left, int right) {
        System.out.println(left + right);
    }
 
    public static void main(String[] args) {
        sum(10, 20);
        sum(20, 40);
    }
 
}
```
refactoring: 기존의 있었던 코드와 정확하게 동작하지만 코드를 더 효율적으로 만든 행위 

- 코드의 양을 줄일 수 있다
- 문제가 생겼을 때 원인을 더 쉽게 찾을 수 있다

```java
package org.opentutorials.javatutorials.object;
 
public class ClaculatorDemo3 {
 
    public static void avg(int left, int right) {
        System.out.println((left + right) / 2);
    }
 
    public static void sum(int left, int right) {
        System.out.println(left + right);
    }
 
    public static void main(String[] args) {
        int left, right;
 
        left = 10;
        right = 20;
 
        sum(left, right);
        avg(left, right);
 
        left = 20;
        right = 40;
 
        sum(left, right);
        avg(left, right);
    }
 
}
```
- 메소드들(sum, avg)가 이 값을 사용하면 코드의 양을 줄일 수 있게 된다

#### 객체화


```java
package org.opentutorials.javatutorials.object;
 
class Calculator{
    int left, right;
      
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    }
      
    public void sum(){
        System.out.println(this.left+this.right);
    }
      
    public void avg(){
        System.out.println((this.left+this.right)/2);
    }
}
  
public class CalculatorDemo4 {
      
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

- Calculator() = 객체
- setOprands = 연산의 대상 값

#### 클래스


- 메소드의 집합 = 객체(작은 프로그램)

- 클래스는 연관되어 있는 변수와 메소드의 집합이다

#### 인스턴스

```java
Calculator c1 = new Calculator();
```
- new Calculator()은 클래스 Calculator를 구체적인 제품으로 만드는 명령
- 만들어진 구체적인 제품을 인스턴스(instance)
    - 클래스 : 설계도
    - 인스턴스 : 제품

```java
Calculator c1
```

- 클래스를 만든다는 것은 사용자 정의 데이터 타입을 만드는 것과 같은 의미

- 클래스를 인스턴스화 할 때는 변수에 담아야 한다는 것과 이 때 사용하는 변수의 데이터 타입은 그 클래스가 된다

```java
public void setOprands(int left, int right){
    this.left = left;
    this.right = right;
}
```

- this는 클래스를 통해서 만들어진 인스턴스 자신을 가리킨다
-  메소드 밖에서 선언한 변수는 인스턴스 내의 모든 메소드에서 접근이 가능하다


![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1878.gif)

- Caculator = 클래스(설계도)

- c1, c2 = 인스턴스(설계도에 따라 만들어진 구체적인 제품)

-  변수 = 상태
-  메소드 = 행동

-  하나의 클래스를 바탕으로 서로 다른 상태를 가진 인스턴스를 만들면 서로 다른 행동을 하게 된다는 것을 알 수 있다