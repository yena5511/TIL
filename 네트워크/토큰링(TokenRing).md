## 토큰링(TokenRing)

- 이너뎃과는 다른 방식의 데이터 네트워크 형태
- 해당 네트워크에서 오직 한 PC, 즉 토큰을 가진 PC만이 네트워크를 통해 데이터를 전송할 수 있다.

![](https://velog.velcdn.com/images/chiwoosong/post/d7b4a1d5-5028-4a5b-8d79-e7de0ec7fde0/image.png)

데이터 전송을 끝낸 A PC가 데이터 전송 차례를 기다리는 B PC에게 Token을 넘김.

#### 장점

- 충돌이 발생하지 않음
- 네트워크에 대한 성능 예측이 쉬움

#### 단점

- 당장 데이터를 보내야하는 PC가 있어도 순서를 기다려야 함