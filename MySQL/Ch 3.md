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

*CREATE TABLE - 테이블 만들기*<br>

```sql
create table people(
	person_id int,
    person_name VARCHAR(10),
    age TINYINT,
    birthday DATE
);
```

*ALTER TABLE - 테이블 변경*<br>

```sql

-- 테이블명 변경
alter table people rename to friends,
-- 컬럼 자료형 변경
change column person_id person_id tinyint,
-- 컬럼명 변경
change column person_name person_nickname VARCHAR(10),
-- 컬럼삭제
drop column birthday,
-- 컬럼 추가
add column is_married TINYINT after age;
```

*DROP TABLE - 테이블 삭제*<BR>

```SQL
drop table friends;
```

#### 2. INSERT INTO - 데이터 삽입

```SQL
insert into people
	(person_id, person_name, age, birthday)
    values(1, '홍길동', 21, '2000-01-31');
```
```SQL
-- 모든 컬럼에 값을 넣을 때는 컬럼명들 생력 가능
insert into people
    values(2, '전우지', 18, '2003-05-12');
```
```SQL
-- 일부 컬럼에만 값 넣기 기능 (NOT NULL 아닐 시)
insert into people
	(person_id, person_name, birthday)
    values (3, '임꺽정', '1995-11-04');
```
```SQL
-- 자료형에 맞지 않는 값은 오류 발생
insert into people
	(person_id, person_name, birthday)
    values (1, '임꺽정', '스물여섯', '1995-11-04');
```
*오류 발생*<br>
![](https://cdn.discordapp.com/attachments/1102264938354978819/1144638071606870086/2023-08-25_232143.png)

```SQL
-- 여러 행을 한 번에 입력 가능
insert into people
	(person_id, person_name, age, birthday)
    values
		(4, '존 스미스', 30, '1991-03-01'),
        (5, '루피 D. 몽키', 15, '2006-12-07'),
        (6, '황비홍', 24, '1997-10-30');
```

#### 3. 테이블 생성시 제약 넣기

```sql
create table people(
	person_id int auto_increment primary key,
    person_name varchar(10) not null,
    nickname varchar(10) unique not null,
    age tinyint unsigned,
    is_married tinyint default 0
);
```

|제약|설명|
|:---|:---|
|AUTO_INCREMENT|새 행 생성기마다 자동으로 1씩 증가|
|PRIMARY KEY|중복 입력 불가, NULL(빈 값)불가|
|UNIQUE|중복 입력 불가|
|NOT NULL|NULL(빈 값)입력 불가|
|UNSIGNED|(숫자일시)양수만 가능|
|DEFAULT|값 입력이 없을 시 기본 값|

💡 PRIMARY KEY (기본키)
- 테이블마다 하나만 가능<BR>
- 기본적으로 인덱스 생성(기본키 행 기준으로 빠른 검색 가능)
- 보통 AUTO_INCREMENT와 함께 사용
- ⭐ 각 행을 고유하게 식별 가능 - 테이블마다 하나씩 둘 것

```SQL
insert into people
	(person_name, nickname, age)
	values('깁철수', '아이언워터', 10);
```
```SQL
insert into people
	(person_name, nickname, age)
    values ('이불가', '임파서블', -2);

```
*오류 발생*<br>
![](https://cdn.discordapp.com/attachments/1102264938354978819/1144644802663690321/2023-08-25_234656.png)
- age가 양수가 아님

```SQL
insert into people
	(person_name, nickname, age, is_married)
    values ('박쇳물', '아이언워터', NULL, 1);
    -- nickname에 null, '아이언수' 넣어보기
```
*오류 발생*<br>
![](https://cdn.discordapp.com/attachments/1102264938354978819/1144644803028603063/2023-08-25_234847.png)
- nickname 겹칠 수 없음
- null도 안 된다
