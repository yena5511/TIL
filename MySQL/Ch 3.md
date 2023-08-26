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

## Lesson 3. 자료형 (+ 예제 데이터)

#### 1. 숫자 자료형

*정수*
|자료형|바이트|SIGNED|UNSIGNED|
|:---|:---|:---|:---|
|TINYINT|1|-128 ~ 127|0 ~ 255|
|SMALLINT|2|-32,768 ~ 32,767|0 ~ 65,535|
|MEDIUMINT|3|-8,388,608 ~ 8,388,607|0 ~ 16,777,125|
|INT|4|-2,147,483,648 ~ 2,147,483,647|0 ~ 4,294,967,295|
|BIGINT|8|-2^63 ~ 2^63 - 1|0 ~ 2^64 - 1|

- 1로 다 나누워 지는 숫자들
- 같은 숫자 자료형도 이 자료형의 데이터 크기가 각각 다름
- SIGNED: 양수와 음수
/ UNSIGNED: 양수

*고정 소수점(Fixed Point)수*
- 좁은 범위의 수 표현 가능, 정확한 값

|자료형|설명|범위|
|:---|:---|:---|
|DECIMAL(  s, d)|실수 부분 총 자릿수(s) & 소수 부분 자릿수 (d)|s 최대 65|

*부동 소수점(Floating Point)수*
- 넓은 범위의 수 표현 가능, 정확하지 않은 값(일반적으로 충분히 정확)

|자료형|표현범위|
|:---|:---|
|FLOAT|-3.402...E+38 ~ -1.175...E-38 , 0 , 1.175...E-38 ~ 3.402...E+38|
|DOUBLE|-1.797...E+308 ~ -2.225E-308 , 0 , 2.225...E-308 ~ 1.797...E+308|

- 고정 소수점: 정확성/ 부동 소수점: 넓은 범위

#### 문자 자료형

*문자열*
|자료형|설명|차지하는 바이트|최대 바이트|
|:---|:---|:---|:---|
|CHAR(s)|고정 사이즈 (남은 글자 스페이스로 채움)|s(고정값)|225|
|VARCHAR(s)|가변 사이즈|실제 글자 수[최대 s]+1[글자수 정보]|65,535|
- 검색시 CHAR가 더 빠름
- VARCHAR 컬럼 길이값이 4글자보다 적을 경우 CHAR로 자동 변환

*텍스트*
|자료형|최대 바이트 크기|
|:---|:---|
|TINYTEXT|255|
|TEXT|65,535|
|MEDIUMTEXT|16,777,215|
|LONGTEXT|4,294,967,295|

#### 3. 시간 자료형

|자료형|설명|비고|
|:---|:---|:---|
|DATE|YYY-MM-DD| |
|TIME|HHH:MI:SS|HHH:-838 ~ 838까지의 시간|
|DATETIME|YYY-MM-DD HH:MI:SS|입력된 시간을 그 값 자체로 저장|
|TIMESTAMP|YYYY-MM-DD HH:MI:SS|MySQL이 설치된 컴퓨터의 시간대를 기준으로 저장|

- 시간 데이터를 가감없이 기록할 때 DATETIME
- 시간 자동기록, 국제적인 서비스를 할 경우 TIMESTAMP 사용

## 참고

- 관계형 데이터베이스는 엑셀처럼 표, 테이블 구조로 된 데이터들을 대량으로 저장하고 관리, 사용하기 위해 만들어짐
- 데이터들이 DB서버의 공강을 계속 차지하기 때문에 저장공간을 최대한 효율적으로 활용

## 실수
- 고정소수점
	- 어떤 실수를 정확히 표현
	- 주어진 자릿수 값을 정수부와 소수부로 나눔


- 부동소수점
	- '부동': 떠서 움직이다
	- 고정되지 않고 떠다닌다
	- 더 넓은 범위의 숫자를 표현하기 위해 사용 됨
	
#### 문자

- CHAR
	- 고정된 공간을 차지
	- 나머지 공간은 스페이스로 채워짐

- VARCHAR
	- 가변적인 공간으 차지
	- 입력되는 글자의 길이에 따라 그에 맞게 공간을 할당
	- 글자 정보에 더해서 글자 길이 정보까지 저장
	- 입력되는 데이터를 통제하는  목적으로 최대 길이수 적음

- TEXT 
	- 게시물의 본문처럼 긴 문자열을 저장할 때
	사용

- BLOB
	- 같은 용량에, 바이너리 데이터를 담는데 사용
	- 이미지 파일 등을 파일로 저장하지 않고 바이너리 데이터로 데이터베이스에 저장하고자 할 때 사용

#### 날짜, 시간

- DATETIME
	- 날짜와 시간, 이 들의 '절대적'인 값

- TIMESTAMP
	- 상대적
	- 시간대의 영향을 받음
	- 행 추가시 값을 안 넣으면 자동으로 현 시간이 입력