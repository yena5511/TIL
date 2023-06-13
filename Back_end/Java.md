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

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    