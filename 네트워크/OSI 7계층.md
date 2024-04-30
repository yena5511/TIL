## OSI 7 계층

OSI 7 계층은 네트워크 통신이 일어나는 과정을 7단계로 나눈 국제 표준화 기구(ISO)에서 정의한 네트워크 표준 모델이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqbCLk%2Fbtryyf3MHwI%2FE20STGOkRn5Y1Lkc88oDP1%2Fimg.png)

#### 1계층 - 물리계층(Physical Layer)

- 주로 전기적, 기계적, 기능적인 특성을 이용해서 통신 케이블로 데이터를 전송하는 물리적인 장비
-  통신 단위 : 비트(Bit)이며 이것은 1과 0으로 나타내어지는, 즉 전기적으로 On, Off 상태
- 장비 : 통신 케이블, 리피터, 허브 등

####  2계층 - 데이터 링크계층(DataLink Layer)

- 물리계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 통신의 흐름을 관리
- 전송되는 단위 : 프레임(Frame)
- 장비 : 브리지, 스위치, 이더넷 등

#### 3계층 - 네트워크 계층(Network Layer)

- 데이터를 목적지까지 가장 안전하고 빠르게 전달
- 이 계층에서 전송되는 단위 : 패킷(Packet)
- 장비 : 라우터

#### 4계층 - 전송 계층(Transport Layer)

- 두 지점간의 신뢰성 있는 데이터를 주고 받게 해주는 역할
- 신호를 분산하고 다시 합치는 과정을 통해서 에러와 경로를 제어

#### 5계층 - 세션 계층(Session Layer)

- TCP/IP 세션 체결, 포트번호를 기반으로 통신 세션 구성

#### 6계층 - 표현 계층(Presentation Layer)

- 전송하는 데이터의 표현방식을 결정
- 파일인코딩, 명령어를 포장, 압축, 암호화

#### 7계층 - 응용 계층(Application Layer)

- 최종 목적지로, 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행