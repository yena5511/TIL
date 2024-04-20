## view

데이터베이스에 존재하는 가상의 테이블을 의미한다.
실제로 행과 열이 존재하지만 데이터를 가지고 있는 것은 아니다.
다른 테이블이나 다른 뷰에 존재하는 데이터를 보여주는 역할만 수행 한다.

#### 필요성

- 사용자마다 특정 객체만 조회할 있음
    -  모든 직원에 대한 정보를 모든 사원이 볼 수 있도록 하면 안 됨
-  복잡한 질의문을 단순화 할 수 있음
- 데이터의 중복을 최소화할 수 있음

#### 생성

```sql
CREATE VIEW 뷰이름 [(열이름 [, ... n])]
AS SELECT 문
```

- '축구'하는 문구가 포함된 자료만 검색
```sql
SELECT *
FROM Book
WHERE bookname LIKE '%축구%';
```

```sql
CREATE VIEW VW_BOOK
AS SELECT *
FROM Book
WHERE bookname LIKE '%축구%';
```
위 문장을 수행하면  VW_BOOK이라는 뷰가 생성되며 일반 테이블처럼 사용할 수 있다.
뷰눈 실제 데이터가 저장되는게 아니라 뷰의 정의가 DBMS에 저장되는 것이다.
만약 추가되는 도서이름에 '축구'라는 문구가 포함되어 있지 않으면 Book테이블에는 존재하지만 뷰에서는 나타나지 않는다.

- Orders테이블에서 고객이름과 도서이름을 바로 확인할 수 있는 뷰를 생성한 후, '김연아' 고객이 구입한 도서의 주문번호, 도서이름, 주문액을 보이시오.
```sql
CREATE VIEW VW_Orders (orderid, custid, name,  bookid, bookname, saleprice, orderdate)
AS SELECT od.orderid, od,custid, cs.name, od.bookid, bk.bookname, od.saleprice, od.orderdate
FROM Orders od, Customer cs, Book bk
WHERE od.custid = cs.custid AND od.bookid = bk.bookid;
```

결과 확인
```sql
SELECT orderid, bookname, saleprice
FROM vw_Orders
WHERE name = '김연아';
```

#### 수정

```sql
CREATE OR REPLACE VIEW 뷰이름 [(열이름 [, ... n])]
AS SELECT 문
```

#### 삭제

```sql
DROP VIEW 뷰이름 [, ... n]
```

#### 장점

- 편리성 및 재사용성
    - 미리 정의된 뷰를 일반 테이블처럼 사용할 있기 때문에 편리
    - 사용자가 필요한 정보만 요구에 맞게 가공하여 뷰로 만들 수 있음
    - 자주 사용되는 질의를 뷰로 미리 정의해 재사용 가능
- 보안성
    - 사용자별로 보안이 필요한 데이터를 제외하고 선별하여 보여줄 수 있음
- 독립성
    - 논리 데이터베이스의 원본 테이블의 구조사 변해도 응용프로그램에 영행을 주지 않도록 하는 논리적 독립성을 제공


#### 단점

- 뷰의 정의 변경 불가
- 삽입, 샂제, 갱신 영산이 제한이 있음
