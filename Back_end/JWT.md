## JWT(JSON Web Token)

`인코딩은 데이터 압축, 다른 형식으로 변환하여 저장 공간 절약, 전송 시간을 줄이는 데 도움이 된다. 
디코딩은 인코딩된 데이터를 원래의 형태로 되돌려 사용자가 이해할 수 있게 만드는 역할`

#### Token
인증을 위해 사용되는 암호화된 문자열

JWT: 모바일 웹의'사용자 인증'을 위해 '인증'에 필요한 정보를 토큰에 담아서 암호화를 시켜 사용하는 인터넷 표준 인증 방식을 의미한다.

- 사용자가 인증에 성공하면 서버는 토큰을 생성해서 클라이언트로 보낸다.
- 토큰은 세션과 마찬가지로 사용자가 보내는 요청에 포함된다.
- 세션 인증에서는 서버가 세션 ID를 저장하고 클라이언트가 쿠키에 실어보낸 세션ID와 대조해서 확인하는 반면, 토큰을 사용하면 요청을 받은 서버는 토큰이 유효한지 확인만 한다.
- 세션 인증에 비해 서버 운영의 효율이 더 좋다.

```
자가 수용저긴 방식
JWT는 필요한 모든 정보를 자체적으로 지니고 있다는 장점이 있다.
이는 JWT에서 발급된 토큰은 기본 정보, 전달할 정보, 검증에 대한 모든 Signature도 포함하고 있으며, 웹 서버 환경에서는 HTTP헤더에 포함을 시키거나 URL의 파라미터로 전달이 가능하다.
```

#### JWT이용한 응용 방안 - 프로세스로 이해하는 JWT

- 클라이언트에서 서버로 최초 호출이 발생하는 경우 Header 내에 토큰의 존재 여부를 체크한다
    - 토큰이 존재하지 않을 경우 '로그인'페이지로 리다이렉트 시켜 로그인 후 토큰을 발급 받는다.
    - 토큰이 존재하는 경우 토큰의 만료시간을 확인한다.
        - 토큰이 만료되었을 경우 재발급을 위해 '로그인'페이지로 리다이렉트 시켜 로그인 후 토큰을 재발급 받는다.
        - 토큰이 유효한 경우 토큰 내의 정보를 확인하여 가져온다.
- 클라이언트에서 서버로 이후 호출이 발생하는 경우 토큰의 만료시간을 확인한다.
    - 토큰이 만료된 경우 '로그인'페이지로 리다이렉트 시켜 로그인 후 토큰을 발급 받는다.
    - 토큰이 만료되지 않은 경우 정상적인 프로세스를 진행한다.

## JWT 구조 이해하기


#### JWT구조
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdUxyF0%2FbtrUfrwwEbG%2Fo4yhu6YCokPAZKRimG77CK%2Fimg.png)

- JWT는 점(.)으로 구분된 세 부분으로 구성되어 있다.

header . payload . signature

이는 서버에서 각각 header, payload, signature를 구성한 값을 인코딩하여서 사용이 된다.

#### JWT의 헤더(header)

- 첫번째, 헤더(header)의 구성은 토큰의 타입과 서명에서 사용할 알고리즘으로 구성되어있다.

```java
{
  "typ": "JWT",
  "alg": "HS256"
}
```

|요소|요소의 key값|요소의 value값|
|:---|:---|:---|
토큰의 타입| typ|JWT|
|토큰의 서명에서 사용할 암호화 알고리즘 종류|alg|HMAC,SHA256,HS256,RS256|

- 구성한 헤더의 JSON은 'Base64Url'로 인코딩 되어서 JSON 웹 토큰의 첫 번째 요소로 사용이 된다.

`헤더 생성 예시`

```java
/**
 * 헤더 값을 생성해주는 메서드
 *
 * @return HashMap<String, Object> 헤더 값
 */
private static Map<String, Object> createHeader() {
    Map<String, Object> header = new HashMap<>();
    header.put("typ", "JWT");
    header.put("alg", "HS256");
    header.put("regDate", System.currentTimeMillis());
    return header;
}
```

#### JWT의 페이로드(payload)

- 두 번째, 페이로드의 구성은 인증을 위해 사용할 실제 정보들(클레임)으로 구성되어 있다.
- 이 클레임의 종류는 등록 클레임, 공개 클레임, 비공개 클레임의 세 가지로 구성되어 있다.

```java
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

```
클레임이란?

페이로드 구성을 담을 key와 value의 한쌍으로 이루어 진 형태의 클레임이라고 한다.
위에 예시로는 "sub":"1234567890"가 하나의 클레임이다.
```

[등록 클레임]
- 필수로 사용되는 정보는 아니지만 JWT의 클레임에서 정의된 키 값을 기준으로 선태하여 사용하는 클레임이다.

|요소 키 값|키 값 설명|예시|
|:---|:---|:---|
|"iss":Issuer Claim| 토큰 발급자 정보|{"iss":"sendApiToken"}|
|"sub":Subject Claim| 토큰 제목 정보| {"sub":"customToken}|
|"aud": Audience Claim|토큰 대상자 정보|	{"aud": "reciveClientToken"}|
|"exp": Expiration Time Claim|토큰 만료시간 정보|{"exp": "1671629718"}|
|"nbf": Not Before Claim|	토큰 활성 날짜|{"nbf": "1671629999"}|
|"lat": Issued At Claim|	토큰 발급 시간|{"lat": "1671629999"}|
|"jti": JWT ID Claim |토큰 식별자|    	{"jti": "jwtidis"}|  

[공개 클레임]

- 사용자가 정의하는 클레임으로 공개용 정보를 위해 사용되어 충돌 방지를 위해 URI 포맷을 이용하는 클레임이다

```java
{
  "https://adjh54.tistory.com/92": true,
}
```

[비공개 클레임]
- 클라이언트와 서버간의 실제 데이터를 전송하려는 것에 대한 정보로 선택하여 사용하는 클레임이다.

```java
{
  "userId": "adjh54",
  "userEmail": "adjh54@gmail.com",
}
```

- 구성한 페이로드(Payload)의 JSON은 ‘Base64Url’로 인코딩 되어서 JSON 웹 토큰의 두 번째 요소로 사용이 된다.


`페이로드 생성 예시`

```java
/**
 * 사용자 정보를 기반으로 클레임을 생성해주는 메서드
 *
 * @param userDto 사용자 정보
 * @return Map<String, Object>
 */
private static Map<String, Object> createClaims(UserDto userDto) {
    // 공개 클레임에 사용자의 이름과 이메일을 설정하여 정보를 조회할 수 있다.
    Map<String, Object> claims = new HashMap<>();

    log.info("userId :" + userDto.getUserId());
    log.info("userNm :" + userDto.getUserNm());

    claims.put("userId", userDto.getUserId());
    claims.put("userNm", userDto.getUserNm());
    return claims;
}
```

#### 서명

- 벡엔드에서 발급되는 서명은 인코딩된 헤더와 인코딩된 페이로드, 비밀 키와 헤더에 저장된 서명에 사용 할 알고리즘을 기반으로 발급이 된다

- 서명 = 인코딩 헤더 + 인코딩 비밀 키 + 비밀 키 + 헤더의 알고리즘

`HMAC SHA256 알고리즘을 기반으로 서명은 아래와 같은 방식으로 구성`
```java
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

- 구성된 서명은 JSON웹 토큰의 마지막 세 번째 요소로 사용이 된다

`서명 생성 예시`

```java
private static String jwtSecretKey = "jwtSecretKey";

/**
 * JWT 서명(Signature) 발급을 해주는 메서드
 *
 * @return Key
 */
private static Key createSignature() {
    byte[] apiKeySecretBytes = DatatypeConverter.parseBase64Binary(jwtSecretKey);
    return new SecretKeySpec(apiKeySecretBytes, SignatureAlgorithm.HS256.getJcaName());
}
```


#### 서버 인증 방법

![](https://velog.velcdn.com/images%2Fkingth%2Fpost%2F10d2beae-96a9-478d-bfd9-258d06941946%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.31.33.png)

- 사용자가 로그인을 한다.
- 서버에서는 계정 정보를 읽어 사용자를 확인 후, 사용자 고유 ID값을 부여한 후, 기타 정보와 함께 payload에 넣는다.
- JWT의 유효기간 설정
- 암호화할 SECRE KEY를 이용해 Access Token을 발급한다.
- 사용자는 Access Token을 받아 저장한 후, 인증이 필요한 요청마다 토큰을 헤더에 실어 보낸다.
- 서버에서는 해당 토큰의 Verify Signature를 SECRET KEY로 복호화한 후, 조작여부, 유효기간을 확인한다.
- 검증이 완료되면, payload를 디코딩하여 사용자의 ID에 맞는 데이터를 가져온다.

세션/쿠키 방식과 가장 큰 차이점은 세션/쿠키는 세션 저장소에 유저의 정보를 넣는 반면, JWT는 토큰안에 유저의 정보들을 넣는다는 점이다.
클라이언트 입당에서는 HTTP헤더에 세션ID나 토큰을 실어서 보내준다는 점에서는 동일하나,
서버측에서는 인증을 위해 암호화를 하나 별도의 저장소를 이용하나의 차이가 발생한다.

#### 장점

- 간편하다 세션/쿠키는 별도의 저장소 관리가 필요하다. 
그러나 JWT는 발급한 후 검증만 하면 되기 때문에 추가 저장소가 필요없다.
이는 Stateless한 서버를 만드는 입장에서는 큰 강점이다. 
여기서 Stateless는 상태(쿠키/세션 정보)를 저장하지 않는 것을 의미한다. 
이는 서버를 확장하거나 유지/보수하는데 유리하다.
- 확정성이 뛰어나다. 토큰기반으로 하는 다른 인증 시스템에 접근이 가능하다. 
예를 들어 Facebook로그인, Goggle로그인 등은 모두 토큰을 기반으로 인증한다. 이에 선택적으로 이름이나 이메일 등을 받을 수 있는 권한도 추가할 수 있다.

#### 문제점

- 이미 발급된 JWT에 대해서는 돌일킬 수 없다. 
세션/쿠키의 경우 만일 쿠키가 악의적으로 이용된다면, 해당하는 세션을 지워버리면 된다.
하지만 JWT는 한번 발급되면 유효기간이 완료될 때까지 계속 사용이 가능하다. 
따라서 악의적인 사용자는 유효기간이 지나기 전까지 정보를 털어갈수 있다.
- Payioad의 정보가 제한적이다. 
Payload는 따로 암호화 되지 않기 때문에 디코딩하면 누구나 정보를 확인할 수 있다. 
(세션/쿠키 방식에서는 유저의 정보가 전부 서버의 저장소에 안전하게 보관) 따라서 유저의 중요한 정보들은 Payload에 넣을 수 없다.
- JWT의 길이. 세션/쿠키 방식에 비해 JWT의 길이는 길다. 
따라서 인증이 필요한 요청이 많아질 수록 서버의 자원 낭비가 발생한다.

#### 해결법

- 기존의 Access Token의 유효기간을 짧게하고 Refresh Token이라는 새로운 토큰을 발급한다.
그렇게 되면 Access Token을 탈취 당해도 상대적으로 피해를 줄일수 있다

## Refresh Token

Access Token(JWT)를 통한 인증 방식의 문제는 제 3자에게 탈취당할 경우 보안에 취약하다는 점이다.
유효기간이 짧은 Token의 경우 그 만큼 사용자는 로그인을 자주해서 새롭게 Token을 발급받아애 하므로 불편하다.
그러나 유효기간을 늘리자면, 토큰을 탈취당했을 때 보안에 더 취약해진다.
"그러면 유효기간을 짧게 하면서 더 좋은 방법은 없을까?" 라는 고민에 의해 탄생하게 된이 Refresf Token이다.


Refresh Token은 Access Token과 똑같은 형때의 JWT이다. 
처음에 로그인을 완료했을 때 Access Token과 동시에 발급되는 Refresh Token은 긴 유효기간을 가지면서, Access Token이 만료됐을 때 새로 발급해주는 열쇠가 된다.


Access Token은 탈취 당하면 정보가 유출되는건 동일하다 .
다만 유효기간이 짧기에 조금 저 안전하다는 뜻이다.

Refresh Token의 유효기간이 만료됐다면, 사용자는 새로 로그인 해야한다.
Refresh Token도 탈취될 가능성이 있기때문에 적절한 유효기간 설정이 필요하다.(보통 2주)


![](https://velog.velcdn.com/images%2Fkingth%2Fpost%2F2b7d73a0-0567-4709-ab37-17e7ec63bbf3%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.31.48.png)

- 사용자가 ID,PW를 통해 로그인
- 서버에서는 회원 DB에서 값을 비교한다(보통 PW는 일반적으로 암호화해서 들어간다)
- 사용자 인증이 되면 서버에서 Access Token, Refresh Token을 보낸다
- 사용자는 Refresh Token은 안전한 저장소에 저장 후, Access Token을 헤더에 실어 요청을 보낸다
- 서버는 Access Token을 검증 후
- 이에 맞는 데이터를 사용자에게 보내준다.
- 시간이 흘러 Access Token이 만료
- 사용자는 만료된 Access Token을 헤더에 실어 요청을 보낸다.
- 서버는 Access Token이 만료된을 확인
- 만료된 토큰임을 알리고 권한없음을 신호로 보낸다 Access Token이 만료될때 마다 9~11 과정을 거칠 필요는 없다. 
Access Token의 Payload를 통해 유효기간을 알수 있다.
따라서 프론트엔드 단에서 API 요청 전에 토큰이 만료 됐다면 바로 재발급 요청 가능
- 사용자는 Refresh Token과 Access Token을 함께 서버로 보낸다.
- 서버는 받은 Access Token이 조작되지 않았는지 확인한후, Refresh Token과 사용자의 DB에 저장되어 있던 Refresh Token을 비교한다.
- 서버는 Token이 동일하고 유효기간도 지나지 않았다면 새로운 Access Token을 사용자에게 보내준다.
- 새로운 Access Token을 헤더에 실어 API요청을 한다.

#### 장점

- 기존의 Acces TOken만 있을 때 보다 안전하다.

#### 문제점

- 구현이 복잡하다.
검증 프로세스가 길기 때문에 자연스럽게 구현하기 힘들어짐(서버/ 클라이언트 모두)
- Access Token이 만료될 때 마다 새롭게 발급하는 과정에서 생기는 HTTP 요청 횟수가 많다. 
이는 서버의 자원 낭비로 이어질수 있다.