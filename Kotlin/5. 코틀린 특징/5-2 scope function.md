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

#### 언제 어떤 scope function을 사용하야 할까

> let: 하나 이상의 함수를 call chain결과로 호출 할 때

```kotlin
val string = listOf("APPLE", "CAR")
string.map{it.length}
    .filter { it > 3 }
    .let { ::println }
```

> let: non-null 값에 대해서만 code block을 실행시킬 때
    
```kotlin
val length = str?.let{
    println(it.uppercase())
    it.length
}
```

>run: 객체 초기화와 반환 값의 게산을 동시에 해야 할 때

```kotlin
val person = Person("이예나", 100).run(personRepository::save)
```
- 객체를 만들어 DB에 바로 저장하고, 그 인스턴스를 활용할 떄

apply 특징: 객체 그 자체가 반환된다
    
>apply: 객체 설정을 할 때에 객체를 수정하는 로직이 call chain중간에 필요 할 때

```kotlin
fun createPerson(
    name: String,
    age: Int,
    hobby: String,
): Person{
    return Person(
        name = name,
        age = age
    ).apply {
        this.hobby = hobby
    }
}
```

also 특징: 객체 그 자체가 반환된다
        
> also: 객체를 수정하는 로직이 call chain 중간에 필요할 때

```kotlin
mutableListOf("one", "two", "three")
    .also { println("four 추가 이전 지금 값: $it") }
    .add("four")
```

> with:   특정 객체를 다른 객체로 변환해야 하는데, 모듈 간의 의존성에 의해 정적 팩토리 혹은 toClass 함수를 만들기 어려울 때

```kotlin
return with(person){
    PersonDto(
        name = name,
        age = age,
    )
}
```
- this를 생략할 수 있어 필드가 많아도 코드가 간결해진다


#### scope function과 가독성

```kotlin
// 1번 코드
if (person != null && person.isAdult){
    view.showPerson(person)
}else{
    view.showError()
}
        
// 2번 코드
person?.tokeIf{it.isAdult}
    ?.let(view::showPerson)
        ?: view.showError()
```

- 1번 코드: 전통적인 if와 else를 활용
- 2번 코드: scope function을 활용한 코틀린스러운 코드

사용 빈도가 적은 관용구는 코드를 더 복잡하게 만들고 이런 관용구들은 한 문장 내에서 조합해 사용하면 복잡성이 휠씬 증가한다        