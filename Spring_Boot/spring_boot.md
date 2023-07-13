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

## 게시글 작성폼 생성

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>게시물 작성폼</title>
</head>

<style>

    .layout {
        width : 500px;
        margin : 0 auto;
        margin-top : 40px;
    }
    .layout > input {
        width : 100%;
        box-sizing : border-box
    }

    .layout >textarea {
        width  : 100%;
        margin-top : 10px;
        min-height : 300px;
    }

</style>

<body>
    <div class="layout">
        <input type = "text">
        <textarea></textarea>
        <button>작성</button>
    </div>
</body>
</html>
```

```java
package com.study.board.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class BoardController {
    @GetMapping("/board/write")//localhost:8080/board/write
    public String boardWriteForm(){
        return "boardwrite";
    }
}
```


## 글 작성 처리

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>게시물 작성폼</title>
</head>

<style>

    .layout {
        width : 500px;
        margin : 0 auto;
        margin-top : 40px;
    }
    .layout input {
        width : 100%;
        box-sizing : border-box
    }

    .layout textarea {
        width  : 100%;
        margin-top : 10px;
        min-height : 300px;
    }

</style>

<body>
    <div class="layout">
        <form action="/board/writepro" method="post">
            <input name="title" type = "text">
            <textarea name="content"></textarea>
            <button type="submit">작성</button>
        </form>
    </div>
</body>
</html>
```


```java
import lombok.Data;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
@Data
public class Board {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY )
    private Integer id;

    private String title;

    private String content;
}
```


```java
package com.study.board.repository;

import com.study.board.entity.Board;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface BoardRepository extends JpaRepository<Board, Integer> {

}```



```java
package com.study.board.service;

import com.study.board.entity.Board;
import com.study.board.repository.BoardRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BoardService {

    @Autowired
    private BoardRepository boardRepository;
    public void write(Board board){

        boardRepository.save(board);
    }
}
```

```java
package com.study.board.controller;

import com.study.board.entity.Board;
import com.study.board.service.BoardService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class BoardController {

    @Autowired
    private BoardService boardService;
    @GetMapping("/board/write")//localhost:8080/board/write
    public String boardWriteForm(){

        return "boardwrite";
    }

    @PostMapping("/board/writepro")
    public String boardWritePro(Board board) {

        boardService.write(board);

        return "";
    }
}
```


## 게시글 리스트


*테스트 데이터 프로시저 생성*
```sql
use board;

DELIMITER $$

CREATE PROCEDURE testDataInsert()
BEGIN
    DECLARE i INT DEFAULT 1;

    WHILE i <= 120 DO
        INSERT INTO board(title, content)
          VALUES(concat('제목',i), concat('내용',i));
        SET i = i + 1;
    END WHILE;
END$$
DELIMITER $$


call testDataInsert;
```

*boardlist.html*
```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>게시글 리스트 페이지</title>
</head>

<style>

    .layout {
      width : 500px;
      margin : 0 auto;
      margin-top : 40px;
    }

</style>
<body>

    <div class="layout">

      <table>
        <thead>
          <tr>
            <th>글번호</th>
            <th>제목</th>
          </tr>

        </thead>
            <tr th:each="board : ${list}">
              <td th:text="${board.id}">1</td>
              <td th:text="${board.title}">제목입니다.</td>
            </tr>
        <tbody>

        </tbody>
      </table>

    </div>

</body>
</html>
```

*BoardService*

```java
package com.study.board.service;

import com.study.board.entity.Board;
import com.study.board.repository.BoardRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BoardService {

    @Autowired
    private BoardRepository boardRepository;
    public void write(Board board){

        boardRepository.save(board);
    }

    public List<Board> boardList(){

        return boardRepository.findAll();
    }
}

```


## 게시글 상세 페이지 생성

*boardview.html*
```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>게시글 상세 페이지</title>
</head>
<body>
<h1 th:text="${board.title}">제목입니다.</h1>
<p th:text="${board.content}">내용이 들어갈 부분입니다.</p>
</body>
</html>
```

*BoardController*
```java
package com.study.board.controller;

import com.study.board.entity.Board;
import com.study.board.service.BoardService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class BoardController {

    @Autowired
    private BoardService boardService;
    @GetMapping("/board/write")//localhost:8080/board/write
    public String boardWriteForm(){

        return "boardwrite";
    }

    @PostMapping("/board/writepro")
    public String boardWritePro(Board board) {

        boardService.write(board);

        return "";
    }

    @GetMapping("/board/list")

    public String boardlist(Model model) {

        model.addAttribute("list", boardService.boardList());
        return "boardlist";
    }

    @GetMapping("/board/view")//localhost:8080/board/view?id>1
    public String boardView(Model model, Integer id){
        model.addAttribute("board", boardService.boardviwe(id));

        return "boardview";
    }
}
```

*BoardService*
```java
package com.study.board.service;

import com.study.board.entity.Board;
import com.study.board.repository.BoardRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BoardService {

    @Autowired
    private BoardRepository boardRepository;
    // 글 작성 처리
    public void write(Board board){

        boardRepository.save(board);
    }

    //게시물 리스트 처리
    public List<Board> boardList(){

        return boardRepository.findAll();
    }

    //특정 게시물 불러오기
    public Board boardviwe(Integer id){

        return boardRepository.findById(id).get();
    }

}

```

*boardlist.html*
```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>게시글 리스트 페이지</title>
</head>

<style>

    .layout {
      width : 500px;
      margin : 0 auto;
      margin-top : 40px;
    }

</style>
<body>

    <div class="layout">

      <table>
        <thead>
          <tr>
            <th>글번호</th>
            <th>제목</th>
          </tr>

        </thead>
            <tr th:each="board : ${list}">
              <td th:text="${board.id}">1</td>
              <td>
                  <a th:text="${board.title}" th:href="@{/board/view(id=${board.id})}"></a>
              </td>
            </tr>
        <tbody>

        </tbody>
      </table>

    </div>

</body>
</html>
```