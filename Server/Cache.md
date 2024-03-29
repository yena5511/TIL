## Cache(캐시)

자주사용하는 데이터나 값을 미리 복사해 놓는 임시 저장소이다.
캐시는 저장 공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다.

캐시를 사용하면 좋은 경우
- 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우(서버의 균일한 API 데이터)
- 반복적으로 동일한 결과를 돌려주는 경우(이미지나 썸네일 등)

Cache에 데이터를 미리 복사해 놓으면 계산이나 접근 시간 없이 더 빠른 속도로 데이터에 접근 할 수 있다.
반복적으로 데이터를 불러오는 경우에, 지속적으로 DBMS 혹은 서버에 요청하는 것이 아니라 Memory에 데이터를 저장하였다가 불러다 쓰는 것이다.
원하는 데이터가 캐시에 존재할 경우 해당 데이터를 반환하며, 이러한 상황을 Cache Hit라고 한다.
하지만 원하는 데이터가 캐시에 존재하지 않을 경우 DBMS 또는 서버에 요청을 해야하며 이를 Cache Miss라고 한다.

#### 캐시의 필요성

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbzw7IJ%2FbtqCaglKbIT%2FUpQYleNiW3pKQsOrzYLR5K%2Fimg.jpg)

 Long Tail 법칙은 20%의 요구가 시스템 리소스의 대부분을 사용한다는 법칙이다. 
 그렇기 때문에 20%의 기능에 Cache를 이용함으로써 리소스 사용량은 대폭 줄이고, 성능은 대폭 향상시킬 수 있다.

 