## OSI 7계층

#### 7. 응용계층
#### 6. 표현 계층: 암축, 암호화
#### 5. 세션 계층: 인증 테크
#### 4. 전송 계층: TCP/ UDP
#### 3. 네트워크 계층: IP
#### 2. 데이터 링크 계층
#### 1. 물리계층: 정비선, 강케이블

#### TCP VS UDP

|TCP|UDP|
|:---|:---|
|신뢰성 높음|신뢰성 X|
|속도 느림|속도 빠름|
|웹 통신| 전화 등 (사람)|


## CIA

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F998B82415BE53B8024)

- 기밀성(Confidentiality)
    - 암호화
    -  노출되면 안 되는 것
    - 인가된 사용자만 자산에 접근할 수 있는 것
    - ex) 암호, 패스워드
- 무결성(Integrity)
    - 변경을 막는 것
    - 권한을 가진 사용자가 인사한 방법으로만 정보를 변경할 수 있도록 접근 통제
- 가용성(Availability) 
    - 내가 원하는 문서에 접근을 할 수 있는지 없는지

- 열쇠 전달의 문제
- 문서는 누구로부터 왔는지

## RSA

- Public key: 공개키, 누구나 알아도 되는 키
- Pricate Key: 개인키, 소유자만 아는 키

1. 공개키의 암호화

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FuO7Pg%2FbtrwmYcCUGk%2FMNrpsbibuf98WPSc2hSGA1%2Fimg.png)

- 공개키는 누구나 아는 키라서 A가 B에게 보낼 때 B의 공개키로 암호화하여 전송한다
- 이 때 공개키로 암호화했기에 B의 비밀키로 암호화
- 해커갸 중간에 탈취 X

2. 개인키 암호화

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzxcxE%2FbtrwmYRdUrr%2F2H9z5SmaJRkiKUmPJMAPo0%2Fimg.png)

- 개인키는 소유자만 가진 키
- A의 개인키로 암호화하며 B에게 전송할 경우 A의 공개키를 복호화할 수 있다
-  모든 이가 A의 공개키를 알기 때문에 해커가 탈취해서 열어볼 수 있다
- 열 수 있기에 보안의 위험성이 있지만 A가 보낸것이 100% 확실하다는 점이 있다