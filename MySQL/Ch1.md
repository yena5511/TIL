**Database**
- 특정 소프트웨어나 프로그램에 종속되지 않고 독립된 정보의 집합 내지 저장소

**Database Management System (DBMS)**
- 관리 시스템을 포함

**Structured Query Language**
- 데이터를 관리할 수 있도록 제공하는 방식
- 구조화 질의 언어
- 도메인 특화 언어

**1.테이블의 모든 내용 보기<BR>**
 *(asterisk)는 테이블의 모든 컬럼을 뜻합니다.

```SQL
SELECT * FROM Customers;
-- 이와 같이 주석을 달 수 있습니다.
```
대소문자 상관없음

**2.원하는 column(열)만 골라서 보기**

```SQL
SELECT CustomerName FROM Customers;
```

```SQL
SELECT CustomerName, ContactName, Country
FROM Customers;
```

*테이블의 컬럼이 아닌 값도 선택할 수 있습니다*

아래 구문의 1과 Hello, NULL을 확인하세요.

```SQL
SELECT
	CustomerName, 1, 'Hello', NULL
FROM Customers
```

**3.원하는 조건의 row(행)만 걸러서 보기**

WHERE 구문 뒤에 조건을 붙여 원하는 데이터만 가져올 수 있습니다.

```sql
SELECT * FROM Orders
WHERE EmployeeID = 3;

```

```sql
SELECT * FROM OrderDetails
WHERE Quantity < 5;
```

**4.원하는 순서로 데이터 가져오기**
ORDER BY 구문을 사용해서 특정 컬럼을 기준으로 데이터를 정렬할 수 있습니다.

|구문|기준|기본
|:---|:---|:---|
|ASC|오름차순|V|
|DESC|내림차순|  |

```sql
SELECT * FROM Customers
ORDER BY ContactName;
```

```sql
SELECT * FROM OrderDetails
ORDER BY ProductID ASC, Quantity DESC;
```

**원하는 만큼만 데이터 가져오기**

LIMIT {가져올 갯수} 또는 LIMIT {건너뛸 갯수}, {가져올 갯수} 를 사용하여, 원하는 위치에서 원하는 만큼만 데이터를 가져올 수 있습니다.

```sql
SELECT * FROM Customers
LIMIT 10;
```

```sql
SELECT * FROM Customers
LIMIT 0, 10;
```

```sql
SELECT * FROM Customers
LIMIT 30, 10;
```

**6.원하는 별명(alias)으로 데이터 가져오기** 

AS를 사용해서 컬럼명을 변경할 수 있습니다.

```sql
SELECT 
	CustomerId AS ID,
    CustomerName AS NAME,
    Address AS ADDR
FROM Customers;

```
```sql
SELECT
  CustomerId AS '아이디',
  CustomerName AS '고객명',
  Address AS '주소'
FROM Customers;
```
 **모두 활용해보기**

 ```sql
 SELECT
	CustomerID AS '아이디',
    CustomerName AS '고객명',
    City AS '도시',
    Country AS '국가'
FROM Customers
WHERE
	City = 'London' OR Country = 'Mexico'
ORDER BY CustomerName
LIMIT 0, 5;

 ```