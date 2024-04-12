##  Bean LifeCycle

해당 객체가 언제 어떻게 생성되어 소멸되기 전까지 어떤 작업을 수행하고 언제, 어떻게 소멸되는지 일련의 과정을 이르는 말이다.

```
스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멀 전 콜백 -> 스프링 종료
```

스프링 의존 관계 주입이 완료되면 스프링 빈애개 콜백 메소드를 통해서 초기화 시점을 알려주는 다양한 기능을 제공한다.
또한 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 제공한다.

Spring Container
- 초기화: Bean을 등록, 생성, 주입
- 종료: Bean 객체들을 소멸

콜백
- 콜백함수를 부를 때 사용되는 용어이며 콜백함수를 등록하면 특정 이벤트가 발생했을 때 해당 메소드가 호출 됨
- 조건에 따라 실행되지 않을 수도 있는 개념

#### 과정 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQ7Epv%2FbtrFwu1aI4R%2Ftm6LUhBs1dkOiyJ7TocoY0%2Fimg.png)

1. 스프링 컨테이너 생성
2. 스프링 빈 생성
3. 의존성 주입
4. 초기화 콜백: 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
5. 사용
6. 소멸전 콜백: 빈이 소멸되기 직전에 호툴
7. 스프링 종료

Bean 생명주기 관리
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdMc5tZ%2FbtrFxjZndp1%2FJK7XXHeEggPlQ1R12pHnJk%2Fimg.jpg)

#### 의존성 주입 과정

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcwgWa4%2FbtryPrbPuYV%2F91fP1CVVaa91LXbkYy0Q40%2Fimg.png)

@Configuration방법을 통해 Been의로 등록

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdE7vZf%2FbtryI58l06O%2FeQPEQsHITv45v1HSpFKad0%2Fimg.png)

- 생성자 주입: 객체의 생성과 의존관게 주입이 동시에 일어남
- Setter, Field주입: 객체의 생성 -> 의존관계 주입으로 라이프 사이클이 나누어져 있음

