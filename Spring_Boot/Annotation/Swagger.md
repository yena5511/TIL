## @ApiOperation

@ApiOperation 어노테이션은 단일 작업을 설명하는 데 사용된다. 
작업은 경로와 HTTP 메서드의 고유한 조합이다.
또한 @ApiOperation 을 사용 하여 성공적인 REST API 호출의 결과를 설명할 수 있다.
즉, 이 어노테이션을 사용하여 일반적인 반환 유형을 지정할 수 있다.

```java
@ApiOperation(value = "Gets customer by ID", 
        response = CustomerResponse.class, 
        notes = "Customer must exist")
@GetMapping("/{id}")
public ResponseEntity<CustomerResponse> getCustomer(@PathVariable("id") Long id) {
    return ResponseEntity.ok(customerService.getById(id));
}
```

#### 값 속성

필수 값 속성에는 작업의 요약 필드가 포함되어 있다.
간단히 말해 작업ㅇ에 대한 간략한 설명을 제공한다.
그러나 이 매개변수를 120자 미만으로 유지해야 한다.

```java
@ApiOperation(value = "Gets customer by ID")
```

#### 메모 속성

메모를 사용하여 작업에 대한 자세한 내용을 제공할 수 있다.
예를 들어 엔드포인트의 제한 사항을 설명하는 텍스트를 배치할 수 있다.

```java
@ApiOperation(value = "Gets customer by ID", notes = "Customer must exist")
```

## @ApiResponse

HTTP상태 코드를 사용하여 오류를 반환하는 것이 일반적이다.
@ApiOperation 어노테이션을 사용하여 오퍼레이션의 가능한 구체적인 응답을 설명할 수 있다.
@ApiResponse 어노테에이션은 작업 및 일반 유혈을 설명하는 반명@ApiResponse
또한 어노테이션은 클래스 수준뿐만 아니라 메소드 수준에서도 적용될 수 있다. 또한 동일한 코드의 @ApiResponse 어노테이션이 메서드 수준에 이미 정의되어 있지 않은 경우에만 클래스 수준에 추가된 어노테이션이 구문 분석됩니다 . 즉, 메서드 어노테이션이 클래스 어노테이션보다 우선합니다.

```java
@ApiResponses(value = {
        @ApiResponse(code = 400, message = "Invalid ID supplied"),
        @ApiResponse(code = 404, message = "Customer not found")})
@GetMapping("/{id}")
public ResponseEntity<CustomerResponse> getCustomer(@PathVariable("id") Long id) {
    return ResponseEntity.ok(customerService.getById(id));
}
```
어노테이션을 사용하여 성공 응답도 지정할 수 있다.

```java
@ApiOperation(value = "Gets customer by ID", notes = "Customer must exist")
@ApiResponses(value = {
        @ApiResponse(code = 200, message = "OK", response = CustomerResponse.class),
        @ApiResponse(code = 400, message = "Invalid ID supplied"),
        @ApiResponse(code = 404, message = "Customer not found"),
        @ApiResponse(code = 500, message = "Internal server error", response = ErrorResponse.class)})
@GetMapping("/{id}")
public ResponseEntity<CustomerResponse> getCustomer(@PathVariable("id") Long id) {
    return ResponseEntity.ok(customerService.getById(id));
}
```
- @ApiResponse 어노테이션 을 사용하여 성공적인 응답을 지정하면 @ApiOperation 어노테이션 내부에 정의할 필요가 없다 .