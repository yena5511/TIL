## Access Token

사용자에 대한 정보를 담고 있ㅁ어서 서비스에 접근 할 수 있는 토큰을 의미
평소 API 통신할 때 사용한다.

## Refresh Token

별다른 정보를 담고 있지는 않는다.
대신 Refresh라는 이름에 걸맞게 액세스 트콘이 만료되었을 때 서버에서 이를 확인하고 새로운 액세스 토큰을 발급해주기 위해 사용

#### AccessToken 한계

JWT는 Stateless이기 때문에 서버에서 상태를 관리하지 않는다.
액세스 토큰으로 JWT를 사용하여 사용자 검증을 진행하면 서버에서 토큰의 상태를 제어할 수 없다.

액세스 토큰만 사용하는 웹사이트는, 이용 중에 토큰이 만료되면 다음 서비스를 이용하려다가 갑자기 로그아웃되거나 오류 메시지를 보게 된다.

- 서버에서 토큰의 상태를 변경할 수 없다.
- 토큰의 유효시간이 짧다면 사용자는 서비스를 이용중에 로그아웃이 된다.
- 토큰의 유효시간이 길다면 보안상의 문제가 된다.

#### 해결

리프레스 토큰을 사용하여 어는정도 해결이 가능하다.
스 토큰의 유효시간을 짧게 하는 대신 유효시간이 긴 리프레시 토큰을 함께 발급하여 액세스 토큰 자체를 계속 갱신한다.

액세스 토큰의 유효시간을 짧게 설정했으니 액세스 토큰을 탈취당했을 때의 위험성도 줄어들게 된다. 
리프레시 토큰을 탈취당했을 때도 리프레시 토큰은 사용자 정보를 담고있지 않으니 상대적으로 안전하다.

![](https://velog.velcdn.com/images/chuu1019/post/8fb38aff-496e-4385-a56a-fbbb58390ba8/image.png)