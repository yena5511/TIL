## TCP/IP(Transmission Control Protocol/ Transmission Control Protocol)

- 인터넷을 포함한 많은 컴퓨터 네트워크에서 세계 표준으로 사용되는 통신 프로토콜
- 오늘날 사용하는 인터넷의 공용어
- 각각의 네트워크에 접속된 호스트들이 고유의 주소를 가지고 있어
자신이 속한 네트워크 뿐만 아니라 다른 네트워크에 연결된 호스트와도 통신이 가능
- 네트워크 통신의 신뢰성과 효율성을 보장
    - 왜냐하면 TCP/IP는 데이터의 정확한 전송을 보장하고, 네트워크 상의 다양한 장치들 간의 통신을 가능하게 하기 때문


#### IP

인터넷 프로토콜(IP, Internet Protocol)은 송신 호스트와 수신 호스트가 패킷 교환 네트워크에서 정보를 주고받는 데 사용하는 정보의 규약이다.
OSI 네트워크 계층에서 호스트의 주소지정과 패킷 분할 및 조립 기능을 담당한다.

**인터넷 프로토콜(IP)의 역할**

- 지정한 IP 주소(IP Address)에 데이터 전달
- 패킷(Packet)이라는 통신 단위로 데이터 전달


전송하고자하는 정보 데이터의 형식을 패킷이라는 하나의 블록 단위로 이루어서 출발지IP에서 목적지 IP까지 패킷을 통해서 전달하고자하는 데이터를 전달한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb4egES%2FbtraoBpAIi2%2F5en9oMl7qzB0gkSUI36Qw1%2Fimg.png)

- IP 주소는 IPv4와 IPv6의 두 가지 형태로 제공
    -  IPv4 주소의 형식은 X.X.X.X 이며, 여기서 각 X는 0-255 범위의 값
    - 사용 가능한 IPv4 주소 풀이 고갈될 수 있다는 우려 때문에 IPv6 프로토콜이 만들어짐
    - IPv4에서 사용하는 32비트 대신 IPv6는 128비트를 사용하여 훨씬 더 큰 잠재적 주소 풀을 제공
    - IPv4는 여전히 인터넷 라우팅의 표준이지만 컴퓨터는 IPv4 및 IPv6 주소를 모두 가질 수 있으며 둘 중 하나를 통해 연결 가능

**DHCP(Dynamic Host Configuration Protocol)**

DHCP(Dynamic Host Configuration Protocol)는 클라이언트에게 동적으로 IP 주소를 할당하는 방법을 제공하는 프로토콜

DHCP의 특징
- 사용중인 주소의 대여기간 갱신(연장) 가능
- 주소의 재사용 가능
- 네트워크에 접속하고자 하는 이동 사용자 지원
