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


## Controller
**BoardController**
```java
package com.study.board.controller;

import com.study.board.entity.Board;
import com.study.board.service.BoardService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.data.web.PageableDefault;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller
public class BoardController {

    @Autowired
    private BoardService boardService;
    @GetMapping("/board/write")//localhost:8080/board/write
    public String boardWriteForm(){

        return "boardwrite";
    }

    @PostMapping("/board/writepro")
    public String boardWritePro(Board board, Model model, MultipartFile file) throws Exception {

        boardService.write(board, file);
        model.addAttribute("message", "글 작성이 완료되었습니다.");

        model.addAttribute("searchUrl", "/board/list");

        return "message";
    }

    @GetMapping("/board/list")

    public String boardlist(Model model,
                            @PageableDefault(page = 0, size = 10, sort = "id", direction = Sort.Direction.DESC) Pageable pageable,
                            String searchKeyword) {
        Page<Board> list = null;

        if(searchKeyword == null){
            list = boardService.boardList(pageable);
        }else {
            list = boardService.boardSearchList(searchKeyword, pageable);
        }

        int nowPage = list.getPageable().getPageNumber() + 1;
        int startPage = Math.max(nowPage - 4, 1);
        int endPage = Math.min(nowPage + 5, list.getTotalPages());
        model.addAttribute("list", list);
        model.addAttribute("nowPage", nowPage);
        model.addAttribute("startPage", startPage);
        model.addAttribute("endPage", endPage);
        return "boardlist";
    }

    @GetMapping("/board/view")//localhost:8080/board/view?id>1
    public String boardView(Model model, Integer id){
        model.addAttribute("board", boardService.boardviwe(id));

        return "boardview";
    }

    @GetMapping("/board/delete")
    public String boardDelete(Integer id){

        boardService.boardDelete(id);
        return "redirect:/board/list";
    }

    @GetMapping("/board/modify/{id}")
    public String boardModify(@PathVariable("id") Integer id, Model model ){
        model.addAttribute("board", boardService.boardviwe(id));
        return "boardmodify";
    }

    @PostMapping("/board/update/{id}")
    public String boardUpdate(@PathVariable("id") Integer id, Board board, MultipartFile file) throws Exception{

        Board boardTemp = boardService.boardviwe(id);
        boardTemp.setTitle(board.getTitle());
        boardTemp.setContent(board.getContent());

        boardService.write(boardTemp, file);

        return "redirect:/board/list";
    }

}

```

## Entity

**Board**
```java
package com.study.board.entity;

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

    private String filename;

    private String filepath;
}

```

## Repository
**BoardRepository**

```java
package com.study.board.repository;

import com.study.board.entity.Board;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface BoardRepository extends JpaRepository<Board, Integer> {

    Page<Board> findByTitleContaining(String searchKeyword, Pageable pageable);
}
```


## Service
**BoardService**
```java
package com.study.board.service;

import com.study.board.entity.Board;
import com.study.board.repository.BoardRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.util.List;
import java.util.UUID;

@Service
public class BoardService {

    @Autowired
    private BoardRepository boardRepository;

    // 글 작성 처리
    public void write(Board board, MultipartFile file) throws Exception{

        String projectPath = System.getProperty("user.dir") + "\\src\\main\\resources\\static\\files";

        UUID uuid = UUID.randomUUID();

        String fileName = uuid + "_" + file.getOriginalFilename();

        File saveFile = new File(projectPath, fileName);

        file.transferTo(saveFile);

        board.setFilename(fileName);
        board.setFilepath("/files/" + fileName);

        boardRepository.save(board);
    }

    //게시물 리스트 처리
    public Page<Board> boardList(Pageable pageable){

        return boardRepository.findAll(pageable);
    }

    public Page<Board> boardSearchList(String searchKeyword, Pageable pageable){
        return boardRepository.findByTitleContaining(searchKeyword, pageable);
    }

    //특정 게시물 불러오기
    public Board boardviwe(Integer id){

        return boardRepository.findById(id).get();
    }


    // 특정 게시글 삭제

    public void boardDelete(Integer id){

        boardRepository.deleteById(id);
    }

}

```

## Templates
**boardlist.html**
```html
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

        <tbody>
        <tr th:each="board : ${list}">
            <td th:text="${board.id}">1</td>
            <td>
                <a th:text="${board.title}" th:href="@{/board/view(id=${board.id})}"></a>
            </td>
        </tr>
        </tbody>
      </table>

        <th:block th:each="page : ${#numbers.sequence(startPage, endPage)}">
            <a th:if="${page != nowPage}" th:href="@{/board/list(page = ${page - 1}, searchKeyword = ${param.searchKeyword})}" th:text="${page}"></a>
            <strong th:if="${page == nowPage}" th:text="${page}" style="color : red" ></strong>
        </th:block>
        <form th:action="@{/board/list}" method="get">
            <input type="text" name="searchKeyword">
            <button type="submit">검색</button>
        </form>
    </div>

</body>
</html>
```

**boardmodify.html**
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
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
  <form th:action="@{/board/update/{id}(id= ${board.id})}" method="post">
    <input name="title" type = "text" th:value="${board.title}">
    <textarea name="content" th:text="${board.content}" ></textarea>
    <button type="submit">수정</button>
  </form>
</div>
</body>
</html>
```

**boardview.html**
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>게시글 상세 페이지</title>
</head>
<body>
<h1 th:text="${board.title}">제목입니다.</h1>
<p th:text="${board.content}">내용이 들어갈 부분입니다.</p>
<a th:href="@{${board.filepath}}">다운받기</a>
<a th:href="@{/board/delete(id=${board.id})}" >글삭제</a>
<a th:href="@{/board/modify/{id}(id = ${board.id})}">수정</a>
</body>
</html>
```
**boardWrite.html**
```html
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
        width : 100%;
        margin-top : 10px;
        min-height : 300px;
    }

</style>

<body>
<div class="layout">
    <form action="/board/writepro" method="post" enctype="multipart/form-data">
        <input name="title" type="text">
        <textarea name="content"></textarea>
        <input type="file" name="file">
        <button type="submit">작성</button>
    </form>
</div>
</body>
</html>
```
**message.html**
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<script th:inline="javascript">

  /*<![CDATA[*/

  var message =[[${message}]];
  alert(message);

  location.replace([[${searchUrl}]]);

  /*]]>*/

</script>
<body>
</body>
</html>
```



## 페이징 처리 2

필요한 변수

- newPage -> 현재 페이지
- startPage -> 블럭에서 보여줄 시작 페이지
- endPage -> 블럭에서 보여줄 마지막 페이지


타임리프 문법
- th:text -> 태그 안에 데이터 출력
- th:each -> 반복문
- th:each="${number:#number(시작번호, 끝번호)}"
-> 시작 번호에서 끝번호까지 반복


## 검색 기능

JPA Repository

- findBy(컬럼이름)
    - 컬럼에서 키워드를 넣어서 찾겠다.
    - 정확하게 키워드가 일치하는 데이터만 검색

- findBy(컬럼이름)Containing
    - 컬럼에서 키워드가 포함된 것을 찾겠다.
    - 키워드가 포함된 모든 데이터 검색