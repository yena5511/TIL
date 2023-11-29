## Hateoas

Hypermedia As The Engine Of Application State의 약자로, 기본적인 아이디어는 

하이퍼미디어를 애플리케이션의 상태를 관리하기 위한 메커니즘으로 사용한다는 것이다.

위의 정의 내용을 이해하고, 구글링을 하다 보면 보이는 정의 내용을 다시 보면 조금은 더 이해하는데 도움이 된다.

Hateoas란 REST Api를 사용하는 클라이언트가 전적으로 서버와 동적인 상호작용이 가능하도록 하는 것을 의미한다.
이러한 방법은 클라이언트가 서버로부터 어떠한 요청을 할 때, 요청에 필요한 URI를 응답에 포함시켜 반환하는 것으로 가능하게 할 수 있다.

#### 사용 예시

`기존의 전형적인 RESTAPI 응답`
```sql
{
"account_id": 12345, 
"balance": "350,000"
}
```

`Hateoas가 적용된 응답 ( URI 정보를 응답에 추가)`
```sql
{
"account id": 12345, 
"balance":"350,000",
 "links": [
        {
            "rel": "self",
            "href" : "http://localhost:8080/accounts/12345"
        },{

            "rel":"withdraw",
            "href" : "http://localhost:8080/accounts/12345/withdraw"
        },{
            "rel": "transfer",
            "href" : "http://localhost:8080/accounts/12345/transfer"
        }
 ]
}
```

- Client 사이드에서는 "rel"의 이름으로 요청 URI를 사용하기 때문에 URI 수정이 발생하더라도 Client 사이드는 수정이 이루어지지 않는다. 
 

