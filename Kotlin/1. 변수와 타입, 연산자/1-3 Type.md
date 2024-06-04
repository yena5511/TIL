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

