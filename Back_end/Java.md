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

