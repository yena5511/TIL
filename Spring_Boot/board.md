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
@ 어노테이션

- @Entity:
클래스 위에 선언하여 이 클래스가 엔티티임을 알려준다
이렇게 되면 JPA에서 정의된 바탕으로 데이터베이스에 테이블을 만들어준다
- @ToString:
해당 클래스에 선언된 필드들을 모두 출력할 수 있는 toString 메서드를 자동으로 생성할 수 있도록 해준다
- @Id, @GeneratedValue: 해당 엔티티의 주요 키(Primary Key, PK)가 될 값을 저장해주는 것이 @Id이다
@GeneratedValue는 이 PK가 자동으로 1씩 증가하는 형태로 생성될지 등을 결정해주는 어노테이션이다

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