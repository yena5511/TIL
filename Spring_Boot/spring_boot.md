## 스프링 부트 게시판 무작정 따라하기 - 소개

1. 개발 환경 세팅
- intellij Community 다운로드
- MariaDB 다운로드
- MySQL Workbench 다운로드


2. 프로젝트 생성
- Intellij Community에서 Spring Boot 프로젝트 생성
- MariaDB Database(스키마) 생성


3. 게시물 작성
- MariaDB Database에 'Board' 테이블 생성
- 게시물 작성 폼 생성
- 게시물 작성 처리


4. 게시물 리스팅
- 게시물 리스트 페이지 생성
- 게시물 리스트 페이지에 저장된 게시물 출력


5. 게시물 삭제
- 게시물 삭제 버튼 생성
- 게시물 삭제 처리


6. 게시물 수정
- 게시물 수정 페이지 생성
- 게시물 수정 처라


7. 게시물 페이징
- 게시물 페이징 처리
- 게시물 리스프 페이지에서 페이징 처리


## 게시판 무작정 따라하기 - 프로젝트 생성

```java
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mariadb://localhost:3306/board
logging.level.org.springframework=debug
logging.level.org.springframework.web=debug
```

```java
package com.study.board.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class BoardController {

    @GetMapping("/")
    @ResponseBody
    public String main(){

        return "Hello World";
    }

}

```

