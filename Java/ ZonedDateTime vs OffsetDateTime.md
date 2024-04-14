##  ZonedDateTime vs OffsetDateTime

#### OffsetDateTime

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FP3j6J%2FbtrJ70IlZ9l%2F4ODjrHVbXHvuRehyloF3r1%2Fimg.png)
- OffsetDateTime(LocalDateTime(날짜 + 시간) + ZoneOffset)을 포함한다,
- Instant와 같이 나노초 정밀도로 타임라인에 순간을 저장한다.
- UTC/ 그리니치에서 오프셋을 추가하면 현지 날짜-시간을 얻을 수 있다.

#### ZonedDateTime

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FboLkgY%2FbtrJ6RFWT77%2FEYdDLcx3MOJPH9S6bqDUck%2Fimg.png)

- ZonedDateTime은 (OffsetDateTime + ZoneRegion)를 포함한다.
- ZoneRegion 은 Timezone을 나타낸 것이라고 보면된다. (ex. Asia/Seoul)
- ZoneRegion은 ZoneId를 상속 받았다.
- 클래스는 타임존 또는 시차 개념이 필요한 날짜와 시간 정보를 나타내기 위해서 사용된다.
- public 생성자를 제공하지 않기 때문에 객체를 생성할 때는 now()나, of()와 같은 정적 메소드를 사용하도록 설계되어 있다.

```java
ZonedDateTime nowSeoul = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
System.out.println("Now in Seoul is " + nowSeoul);
```