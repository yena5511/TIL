#### @Secured

@Secured 어노테이션은 권한이 필요한 부분에 선언 할 수 있는데 Class나 Method 단위까지 지정을 할 수 있다. 
예를 들면 ROLE_ADMIN만 접근시킬 메서드가 있다면 해당 메서드위에 @Secured 어노테이션을 선언해주기만 하면 된다.

```java
ex1)접근 가능한 권한이 하나 일때

@Secured("ROLE_ADMIN")
public String encoding(String str){
 return encoder.encodePassword(str,null);
}

ex2)접근 가능한 권한이 두개 이상일때

@RequestMapping(value = "/checkAuth", method = RequestMethod.GET)
@Secured({"ROLE_ADMIN","ROLE_USER"})
public String checkAuth(Locale locale, Model model, Authentication auth) {
 UserDetailsVO vo = (UserDetailsVO) auth.getPrincipal();
 logger.info("Welcome checkAuth! Authentication is {}.", auth);
 logger.info("UserDetailsVO == {}.", vo);
 model.addAttribute("auth", auth );
 model.addAttribute("vo", vo );
 return "checkAuth";
}
```
- 적용후 해당 메서드에 권한이 없는 사용자로 접근을 하게 되면 권한이 필요하다는 403에러가 뜬다.


#### @PreAuthorize

요청이 들어와 함수를 실행하기 전에 권한을 검사한다.
Spring EL(표현식)을 사용할 수 있고, AND나 OR 같은 표현식을 사용할 수 있다.

```
// ROLE_USER와 ROLE_ADMIN 두 개의 권한을 가져야 접근 가능
@PreAuthorize("hasRole('ROLE_USER') and hasRole('ROLE_ADMIN')")
 ```

 ##### @PostAuthorize
 
 함수를 실행하고 클라이언트한테 응답을 하기 직전에 권한을 검사한다.

#### @AuthenticationPrincipal

세션 정보 UserDetails에 접근할 수 있는 어노테이션

`@AuthenticationPrincipal`은 `UserDetails`타입을 가지고 있음 -> `UserDetails`타입을 구현한 `PrincipalDetails`클래스를 받아 User object를 얻는다.

- userDetails(PrincipalDetails 타입).getUser()

따라서 로그인 세션 정보가 필요한 컨트롤러에서 매번 @AuthenticationPrincipal로 세션 정보를 받아서 사용한다. 

- @AuthenticationPrincipal`UserAdapter`타입
- 로그인 세션 정보를 어노테이션을 간편하게 받을 수 있다
- UserDetailsService에서 return한 객체를 파라미터로 직접 받아 사용할 수 있다
- name 뿐만 아니라 다양한 정보를 받을 수 있다
- 중복 코드를 효율적으로 줄일 수 있다


