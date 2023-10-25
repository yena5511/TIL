## UserDetails

- 인증에 성공하여 생성된 UserDetails 객체는 Authentication 객체를 구현한 UsernamePasswordAuthenticationToken을 생성하기 위해 사용된다

|메서드|리턴 타입|설명|기본값|
|:---|:---|:---|:---|
|getAuthorities()|Collection<? extends GrantedAuthority>|계정의 권한 목록을 리턴| |
|getPassword()|String|계정의 비밀번호를 리턴| |
|getusername()|String|게정의 고유한 값을 리턴(ex: DB PK값, 중복이 없는 이메일 값)| |
|isAccountNonExpired()|boolean|계정의 만료 여부 리턴| true(만료 안됨)|
|isAccountNonLocked()|boolean|계정의 잠김 여부 리턴|true(잠기지 않음)|
|isCredentialsNonExpired()|boolean|비밀번호 만료 여부 리턴|true(만료 안됨)|
|isEnabled()|boolean|계정의 활성화 여부 리턴|true(활성화 됨)|

여기서 잘 봐야하는 메서드가 getUsername()이다.


```java
@Getter
public class CustomUserDetails implements UserDetails, Serializable {

    private static final long serialVersionUID = 174726374856727L;

    private String id;	// DB에서 PK 값
    private String loginId;		// 로그인용 ID 값
    private String password;	// 비밀번호
    private String email;	//이메일
    private boolean emailVerified;	//이메일 인증 여부
    private boolean locked;	//계정 잠김 여부
    private String nickname;	//닉네임
    private Collection<GrantedAuthority> authorities;	//권한 목록
	
    
    /**
    * 해당 유저의 권한 목록
    */
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
           return authorities;
    }

	/**
    * 비밀번호
    */
	@Override
    public String getPassword() {
        return password;
    }


	/**
    * PK값
    */
    @Override
    public String getUsername() {
        return id;
    }

    /**
     * 계정 만료 여부
     * true : 만료 안됨
     * false : 만료
     * @return
     */
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    /**
     * 계정 잠김 여부
     * true : 잠기지 않음
     * false : 잠김
     * @return
     */
    @Override
    public boolean isAccountNonLocked() {
        return locked;
    }

    /**
     * 비밀번호 만료 여부
     * true : 만료 안됨
     * false : 만료
     * @return
     */
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }


    /**
     * 사용자 활성화 여부
     * ture : 활성화
     * false : 비활성화
     * @return
     */
    @Override
    public boolean isEnabled() {
        //이메일이 인증되어 있고 계정이 잠겨있지 않으면 true
        return (emailVerified && !locked);
    }

}
```