## 코틀린의 scope function

#### scope function 이란

- scope : 영역
- function : 함수
- scope function : 일시적인 영역을 형성하는 함수

> 기존 코드
```kotlin
fun printPerson(person: Person?){
    
    if (person != null){
        println(person.name)
        println(person.age)
    }
}
```

> let 
```kotlin
fun printPerson(person: Person?){
    person?.let {
        println(it.name)
        println(it.age)
    }
   
}
```
- Safe Call(?.)을 사용: person이 null아 아닐때에 let을 호출

- let
    - scope function의 종류
    - 확장함수 
    - 람다를 받아, 람다 결과를 반환한다

람다를 사용해 일시적인 영억을 만들고 코드를 더 간결하게 만들거나, method chaning에 활용하는 함수를 scopr function이라고 한다
