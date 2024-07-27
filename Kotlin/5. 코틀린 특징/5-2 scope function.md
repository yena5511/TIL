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

#### scope function의 종류

- let: 람다의 결과
- run: 람다의 결과
- also: 객체 그 결과
- apply: 객체 그 결과
- with(파라미터, 람다): this를 사용해 접근하고, this는 생략 가능하다

```kotlin
val value1 = person.let{
    it.age
}

val value2 = person.run{
    this.age
}

val value3 = person.also{
    tt.age
}

val value4 = person.apply{
    this.age 
}
```

- this: 생략이 가능한 대신, 다른 이름을 붙일 수 없다
    - let은 일반 함수를 받는다
- it: 생략이 불가능한 대신, 다른 이름을 붙일 수 있다
    - run은 확장 함수를 받는다
```kotlin
val value1 = person.let{ p ->
    p.age
}

val value2 = person.run{
    age
}
```

> with
```kotlin
val person = Person("이예나", 100)
with(person){
    println(name)
    println(this.age)
}
```
