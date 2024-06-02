## null을 다루는 방법

#### null체크

```kotlin
   fun startWithA1(str: String?): Boolean{
        if(str == null){
            throw IllegalArgumentException("null값이 들어왔습니다")
        }
        return str.startsWith("A")
    }
```

```kotlin
    fun startWithA2(str: String?): Boolean? {
        if(str == null){
         return null
        }
        return str.startsWith("A")
    }
```

```kotlin
    fun startWithA3(str: String?): Boolean {
        if(str == null) {
            return false
        }
        str.startsWith("A")
    }
```

- null이 가능한 타입을 완전히 다르게 취급함

#### safe call과 Elvis 연산자

- 코틀린에서는 null이 가능한 타입을 완번히 다르게 취금한다

- Safe Call

null이 아니면 실행하고 null이면 실행하지 않는다 (그대로 null)

```kotlin
val str: String? = "AZBC"
println(str?.length)
```
결과: 4

```kotlin
val str: String? = null
println(str?.length)
```
결과: null

-  Elvis 연산자

앞의 연산 결과가 null이면 뒤의 값을 사용

```kotlin
val str: String? = null
println(str?.length ?: 0)

```
결과: 0

- 널 아님 단언

nullable type이지만, 아무리 생각해도 null이 될 수 없는 경우

!! => 절대 null이 아님

```kotlin
fun main(){
    println(startWith("ABC"))
}
fun startWith(str: String?):Boolean{
    return str!!.startsWith("A")
}
```
결과: true

만약 Null인경우

```kotlin
fun main(){
    println(startWith(null))
}
fun startWith(str: String?):Boolean{
    return str!!.startsWith("A")
}
```

결과: NullPointrException

- 플랫폼 타입

 kotlin에서 java 코드를 가져다 사용할 때 어떻게 처리할까


> java 코드
```java
public class Person{
    
    private final String name;
    
    public Person(String name){
        this. name = name;
    }
    @Nullable
    public String getName(){
        return name;
    }
}
```

> kotlin 코드
```kotlin
    fun main(){
        val person = Person("이예나")
    }
    
    fun startWithA(str: String):Boolean{
        return str.startsWith("A")
    }

```

결과: 오류 발생

> java 코드
```java
public class Person{
    
    private final String name;
    
    public Person(String name){
        this. name = name;
    }
    @NotNull
    public String getName(){
        return name;
    }
}
```
결과: kotlin메서 실행 가능

> 플랫폼 타입: 코틀린에서 Null 관련 정보를 알 수 없는 타입 Runtime시 Exception이 날 수 있다
