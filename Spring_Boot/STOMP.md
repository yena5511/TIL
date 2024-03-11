## STOMP (Simple Text Oriented Messaging Protocol)

- TCP 또는 WebSocket 같은 양방향 네트워크 프로토콜 기반으로 동작
- Message Payload에는 Text or Binary 데이터를 포함 할 수 있다.
- 간단한 메시지를 전송하기 위한 프로토콜로 메시지 브로커를와 publisher - subscriber 방식을 사용한다.
- 

#### Spring WebSocket STOMP

- Spring에서 지원하는 stomp
- 메세지 송수신에 대한 처리가 명확하게 정의할 수 있다.
- WebSocketHandler 를 직업 구현할 필요 없이, @MessagingMapping 어노테이션을 사용해 메시지 발행 시 엔드포인트를 별로도 분리해서 관리할 수 있다.

#### HTTP 통신의 특징

1. 비연결성: 연결은 맺고 요청을 하고 응답을 받는면 연결은 끊어버린다.
2. 무상태성: 서버가 클라이언트의 상태를 가지고 있지 않는다.

#### STOMP 동작 흐름

![](https://velog.velcdn.com/images/limsubin/post/12d80804-86a7-4ab1-88a6-58988fd58b13/image.png)