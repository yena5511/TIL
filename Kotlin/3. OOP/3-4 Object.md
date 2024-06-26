## 코틀린에서 object 키워드를 다루는 법

#### static 함수와 변수

> class Person 
```kotlin
class Person private constructor(

    var name: String,
    var age: Int

){

    companion object{
        val MIN_AGE = 1
        fun newBaby(name: String): Person{
            return Person(name, MIN_AGE)
        }
    }

}

fun main(){

}
```
- static대신  companion object를 사용


static: 클래스가 인스턴스화 될 때 새로운 값이 복제되는게 아니라 정적으로 인스턴스끼리의 값을 공유

companion object: 클래스와 동행하는 유일한 오브젝트       


> class Person 
```kotlin
 companion object{
        private const val MIN_AGE = 1
        fun newBaby(name: String): Person{
            return Person(name, MIN_AGE)
        }
    }
```

const: 컴파일시에 변수가 할당된다
        
진짜 상수에 붙이기 위한 용도
기본 타입과 String에 붙일 수 있음

> 자바와 다른점 

companion object, 즉 동반객체도 하나의 객체로 간주된다
때문에 이름을 붙일 수도 있고, interface를 구현할 수도 있다

```kotlin
 companion object Factory: Log{
        private const val MIN_AGE = 1
        fun newBaby(name: String): Person{
            return Person(name, MIN_AGE)
        }

        override fun log(){
            println("나는")
        }
    }
```

#### 싱글톤

```kotlin
fun main(){

    println(Singleton.a)
    Singleton.a += 10
    println(Singleton.a)

}


object Singleton{
    var a: Int = 0
}
```


#### 익명 클래스
        
특정 인터페이스나 클래스를 상속받은 구현체를 일회성으로 사용할 때 쓰는 클래스

```kotlin
fun main(){
    moveSomething(object : Movable {
        override fun move(){
            println("무브 무브")
        }
        
        override fun fly(){
            println("날다날다")
        }
    })
}

private fun moveSomething(movable: Movable){
    movable.move()
    movable.fly()
}
```

Java에서는 new타입이름() Kotlin에서는 object : 타입이름