## Swagger

Swagger는 애플리케이션의 RESTful API 문서를 자동으로 구성하는 특수 도구이다.

그 장점은 애플리케이션의 모든 엔드포인트를 살펴볼 수 있을 뿐만 아니라 요청을 보내고 응답을 수신하여 작동 중인 엔드포인트를 즉시 테스트할 수 있다는 사실에 있다.

- Swagger는 개발한 Rest API를 문서화 한다.
- 문서화된 내용을 통해 관리 & API 호출을 통한 테스트를 가능케 한다. 
- API Test 할때 많이 사용되는 PostMan과 비슷하다. 

`의존성 추가`
```java
// swagger
	compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.9.2'
	compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.9.2'
```

`SwaggerConfig`
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.peterica.swagger"))
//                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build()
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfo(
                "Swagger Test REST API",
                "API의 설명을 여기에 기제한다.",
                "API 버젼은 여기",
                "팀의 서비스 URL",
                new Contact("앱의 이름", "www.example.com", "ilovefran.ofm@gmail.com"),
                "License of API", "license URL", Collections.emptyList());
    }

}
```

`Contoller`

```java
@RestController
@RequiredArgsConstructor
public class DemoController {

    private final TestService testService;

    @GetMapping("/")
    @ApiOperation(httpMethod = "GET", value = "pingPong", notes = "pingPong",
            response = String.class, tags = "시스템 모니터링")
    public String pong(){
        return "pong";
    }

    @GetMapping("/dbTest")
    @ApiOperation(httpMethod = "GET", value = "Mybatis Test", notes = "Mybatis Test", 
    		tags = "Mybatis테스트")
    public List<String> dbTest(){
        return testService.getTest();
    }
}
```