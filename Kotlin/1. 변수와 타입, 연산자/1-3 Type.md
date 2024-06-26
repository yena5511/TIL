## Type을 다루는 방법

#### 기본타입

코틀린에서는 선언된 기본값을 보고 타입을 추론한다

```kotlin
val number1 = 3 //Int
val number2 = 3L //Long

val number3 = 3.0f //Float
val number4 = 3.0 //Double
```

- java: 기본 타입간의 변환은 암시적으로 이루어질 수 있다.
- kotlin: 기본 타입간의 변환은 명시적으로 이루어져야 한다.

>java
```java
int number1 = 4;
long number2 = number1;

System.out.println(number1+number2)
```
int 타입의 값이 long타입으로 암시적으로 변경되었다.
Java에서 더 큰 타입으로는 암시적 변경이 된다.

>kotlin
```kotlin
val number5 = 4
val number6: Long = number5 // Type mismatch

println(number5 + number6)
```
결과: 오류 발생

kotlin에서는 암시적 타입 변경이 불가능하다

> 해결
```kotlin
val number5 = 4
val number6: Long = number5 // Type mismatch

println(number5 + number6)
```

#### 타입 캐스팅
    
```java
public static void printAgeIfPerson(Object obj){
        if(obj instanceof Pserson){
            Person person = (Person) obj;
            System.out.println(person,getAge());
        }
    }
```
- instanceof: 변수가 주어진 타입이면 true, 그렇지 않으면 false
- (타입): 주어진 변수를 해당 타입으로 변경한다


```kotlin
  fun printAgeIfPerson(obj: Any){
       if (obj is Person){
           val person = obj as Person
           println(pserson.age)
       }
   }
```
- is: 자바의 instanceof
- as Person: 자바의 (Person)obj, 생략가능

> 스마트 캐스트
```kotlin
   fun printAgeIfPerson(obj: Any){
       if (obj is Person){
           println(pserson.age)
       }
   }

```
가능

만약 null값이 들어간다면

```kotlin
fun main(){
    printAgeIfPerson(null)
}   
   
fun printAgeIfPerson(obj: Any?){
    val person = obj as? Person
    println(person?.age)
   }
```

#### 특이한 타입 3가지

Any

- Java의 Object 역할(모든 객체의 최상위 타입)
- 모든 Peimitive Type의 최상의 타입도 Any이다
- Any자체로는 null을 포함할 수 없어 null을 포함하고 싶다면, Any?로 표현
- Any에 equals/hashCode/toString 존재

Unit

- Unit은 Java의 void외 동일한 역할
- void와 다르게 Unit은 그 자체로 타입 인자로 사용 가능하다
- 함수형 프로그래밍에서 unir은 단 하나의 인스턴스만 갖는 타입을 의미
  - 즉 코틀린의 Unit은 실제 존재하는 타입이라는 것을 표현

Nothing

- Nothing은 함수가 정사적으로 끝나지 않았다는 사실을 표현하는 역할
- 무조건 예외를 반환하는 함수
- 무한 루프 함수 등
    
```kotlin
fun fail(message: String): Nothing{
    throw IllegalArgumentException(message)
}
```

#### String interpolation/ String indexing

>java
```java
val person = Person("이예나", 18)
System.out.println(Sting.format("이름 : %s", person.name))
```

>kotlin
```kotlin
val person = Person("이예나", 18)
println("이름 : ${person.name}")
```
- ${변수}를 사용하면 값이 들어간다

```kotlin
val name = "이예나"
println("이름 : $name")
```
- $변수를 사용할 수도 있다 ( {}생략가능 )
- 변수 이름만 사용하더라도 ${변수}를 사용하는 것이 더 좋다
    - 가독성
    - 일괄변환
    - 정규식 활용


> 여러 줄에 걸친 문자열 작성 큰따옴표 세 개("""""")
```kotlin
 val str = """
          ABC
          DEF
          ${name}
        """.trimIndent()
        println(str)
```

> 특정 문자열 가져오기

```kotlin
val str = "abcd"
val ch = str[1]
```