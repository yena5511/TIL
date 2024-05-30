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