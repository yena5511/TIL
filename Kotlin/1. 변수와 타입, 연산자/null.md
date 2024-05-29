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