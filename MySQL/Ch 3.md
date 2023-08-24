## Lesson 1. MySQL 설치하기

#### MySQL Workbench 사용하기

- localhost로 연결하기
- 설정했던 비밀번호 root 계정 접속
- 데이터베이스 생성
```sql
CREATE SCHEMA `mydatabase` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci ;
```
|항목|사용 이유|
|:---|:---|
|utf8mb4|한글을 포함한 전세계 문자 + 이모티콘 사용 가능|
|utf8mb4_general_ci|가장 정확하지는 않지만 정렬 속도 빠름|

```sql
-- 데이터베이스 삭제 명령어
DROP DATABASE `mydatabase`;
```

#### Salkila Datebase 설치

1. File > Open SQL Script > ...sakila-schema.sql
2. File > Open SQL Script > ...sakila-data.sql

```sql
select * from actor limit 100;
```

```sql
SELECT
	F.title AS FilmTitle,
    CONCAT(A.first_name,' ' , A.last_name) AS ActorName
from film F
left join film_actor FA
	on F.film_id = FA.film_id
left join actor A
	on A.actor_id = FA.actor_id
limit 100;

```

#### CLI로 실행해보기

- 터미널 또는 파워쉘에서 MySQL이 설치된 폴더로 이동

```
./mysql -u root -p
```

## Lesson 2. 테이블 만들고 데아터 입력하기

#### 테이블 생성/ 수정/ 삭제