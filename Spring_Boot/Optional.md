## Optional

'null일 수도 있는 객체'를 감싸는 일종의 Wrapper 클래스이다.

```java
Optional<T> optional
```

- optional 변수 내부에는 null이 아닌 T 객체가 있을 수도 있고 null이 있을 수도 있다.
따라서, Optional 클래스는 여러가지 API를 제공하여 null일 수도 있는 객체를 다룰 수 있도록 돕는다.

#### NPE(NullPointerException)

null 여부를 검사해야 하는데, null 검사를 해야하는 변수가 많은 경우 코드가 복잡해지고 번거롭다.
그래서 null 대신 초기값을 사용하길 권장하기도 한다.

```java
List<String> names = getNames();
names.sort(); // names가 null이라면 NPE가 발생함

List<String> names = getNames();
// NPE를 방지하기 위해 null 검사를 해야함
if(names != null){
    names.sort();
}
출처: https://mangkyu.tistory.com/70 [MangKyu's Diary:티스토리]
```

#### Optional 객체 생성

```java
Optional<User> result = userRepository.findById(userId);
```

#### Optional 객체 생성

```java
f(result.isPresent()) {
                return result.get();
            }else{
                return result.orElse(null);
```
- get(): Optional 내부에 담긴 객체를 반환한다.
만약, null인 객체라면 NoSuchElementException이 발생한다.
따라서, isPresent()로 체크한 후에 이 get메서드를 사용

- orElse(): 있으면 값을 반환하고, 그렇지 않으면 다른 값을 반환

orElseGet () : 존재하는 경우 값을 반환하고, 그렇지 않으면 other를 호출 하고 해당 호출의 결과를 반환