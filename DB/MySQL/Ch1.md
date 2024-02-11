**Database**
- 특정 소프트웨어나 프로그램에 종속되지 않고 독립된 정보의 집합 내지 저장소

**Database Management System (DBMS)**
- 관리 시스템을 포함

**Structured Query Language**
- 데이터를 관리할 수 있도록 제공하는 방식
- 구조화 질의 언어
- 도메인 특화 언어

## Lesson 1. SELECT 전반 기능 훑어보기

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

 ## Lesson 2. 각종 연산자들


**1.사칙연산**

|연산자|의미|
|:---|:---|
|+,-,*,/|각각 더하기, 빼기, 곱하기, 나누기|
|%, MOD|나머지|

```sql
SELECT 1 + 2;
```

```sql
SELECT 5 - 2.5 AS DIFFERENCE;
```

```sql
SELECT 3 * (2 + 4) / 2, 'Hello';
```

```sql
SELECT 10 % 3;
```

*문자열에 사칙연산을 가하면 0으로 인식*

```sql
SELECT 'ABC' + 3;
```
![](https://cdn.discordapp.com/attachments/1083231554521813105/1134373215024726077/125bb982f109a68f.png)
```sql
SELECT 'ABC' * 3;
```
![](https://cdn.discordapp.com/attachments/1083231554521813105/1134373232196210698/248ba80417ff6c17.png)
```sql
SELECT '1' + '002' * 3;

-- 숫자로 구성된 문자열은 숫자로 자동인식
```
![](https://cdn.discordapp.com/attachments/1083231554521813105/1134373267461914725/2023-07-28_135140.png)
```sql
SELECT
	OrderID, ProductID,
    OrderID + ProductID AS SUM
FROM OrderDetails;
```

```sql
SELECT
  ProductName,
  Price / 2 AS HalfPrice
FROM Products;
```

**2.참/거짓 관련 연산자**

```SQL
SELECT TRUE, FALSE;
```
```SQL
SELECT !TRUE, NOT 1, !FALSE, NOT FALSE;
```
💡 MySQL에서는 TRUE는 1, FALSE는 0으로 저장된다
```SQL
SELECT 0 = TRUE, 1 = TRUE, 0 = FALSE, 1 = FALSE;
```

```SQL
SELECT * FROM Customers WHERE TRUE;
```

```SQL
SELECT * FROM Customers WHERE FALSE;
```

|연산자|의미|
|:---|:---|
|IS|양쪽이 모두 TRUE 또는 FALSE|
|IS NOT|한쪽은 TRUE. 한쪽은 FALSE|

```SQL
SELECT TRUE IS TRUE;
```
```SQL
SELECT TRUE IS NOT FALSE;
```
```SQL
SELECT (TRUE IS FALSE) IS NOT TRUE;
```

|연산자|의미|
|:---|:---|
|AND, &&|양쪽이 모두 TRUE일 때만 TRUE|
|OR, ㅣㅣ|한쪽이 TRUE면 TRUE|

```SQL
SELECT TRUE AND FALSE, TRUE OR FALSE;
```
```SQL
SELECT 2 + 3 = 6 OR 2 * 3 = 6;
```
```SQL
SELECT * FROM Orders
WHERE
  CustomerId = 15 AND EmployeeId = 4;
```
```SQL
SELECT * FROM Products 
WHERE
  ProductName = 'Tofu' OR CategoryId = 8;
```
```SQL
SELECT * FROM OrderDetails
WHERE
  ProductId = 20
  AND (OrderId = 10514 OR Quantity = 50);
```

|연산자|의미|
|:---|:---|
|=|양쪽 값이 같음|
|!=, <>|양쪽 값이 다름|
|>, <|(왼쪽, 오른쪽) 값이 더 큼|
|>=, <=|(왼쪽, 오른쪽)값이 같거나 더 큼|

```SQL
SELECT 1 = 1, !(1 <> 1), NOT (1 < 2), 1 > 0 IS NOT FALSE;
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1134813577597947967/2023-07-29_203259.png)
```SQL
SELECT 'A' = 'A', 'A' != 'B', 'A' < 'B', 'A' > 'B';
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1134813577954476043/2023-07-29_203359.png)
```SQL
SELECT 'Apple' > 'Banana' OR 1 < 2 IS TRUE;
```

❗ MySQL의 기본 사칙연산자는 대소문자 구분을 하지 않습니다.
```SQL
SELECT 'A' = 'a';
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1134813578248081428/2023-07-29_203439.png)

```SQL
SELECT
  ProductName, Price,
  Price > 20 AS EXPENSIVE 
FROM Products;
```
💡 테이블의 컬럼이 아닌 값으로 선택하기. 
```SQL
SELECT
  ProductName, Price,
  NOT Price > 20 AS CHEAP 
FROM Products;
```

|연산자|의미|
|:---|:---|
|BETWEEN{MIN} AND {MAX}|두 값 사이에 있음|
|NOT BETWEEN{MIN} AND {MAX}|두 값 사이가 아닌 곳에 있음|

```SQL
SELECT 5 BETWEEN 1 AND 10;
```
```SQL
SELECT 'banana' NOT BETWEEN 'Apple' AND 'camera';
```
```SQL
SELECT * FROM OrderDetails
WHERE ProductID BETWEEN 1 AND 4;
```
```SQL
SELECT * FROM Customers
WHERE CustomerName BETWEEN 'b' AND 'c';
```

|연산자|의미|
|:---|:---|
|IN(...)|괄호 안의 값들 가운데 있음|
|NOT IN(...)|괄호 안의 값들 가운데 없음|

```SQL
SELECT 1 + 2 IN (2, 3, 4) 
```
```SQL
SELECT 'Hello' IN (1, TRUE, 'hello') 
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1134813578533290024/2023-07-29_203923.png)
```SQL
SELECT * FROM Customers
WHERE City IN ('Torino', 'Paris', 'Portland', 'Madrid') 
```

|연산자|의미|
|:---|:---|
|LIKE'...%...'|0~N개 문자를 가진 패턴|
|LIKE'..._....'|_갯수만큼의 문자를 가진 패턴|

```SQL
SELECT
  'HELLO' LIKE 'hel%',
  'HELLO' LIKE 'H%',
  'HELLO' LIKE 'H%O',
  'HELLO' LIKE '%O',
  'HELLO' LIKE '%HELLO%',
  'HELLO' LIKE '%H',
  'HELLO' LIKE 'L%'
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1134813578835284008/2023-07-29_204123.png)
```SQL
SELECT
  'HELLO' LIKE 'HEL__',
  'HELLO' LIKE 'h___O',
  'HELLO' LIKE 'HE_LO',
  'HELLO' LIKE '_____',
  'HELLO' LIKE '_HELLO',
  'HELLO' LIKE 'HEL_',
  'HELLO' LIKE 'H_O'
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1134813579112091680/2023-07-29_204307.png)
```SQL
SELECT * FROM Employees
WHERE Notes LIKE '%economics%'
```
```SQL
SELECT * FROM OrderDetails
WHERE OrderID LIKE '1025_'
```

**총정리**
|연산자|의미|
|:---|:---|
|+,-,*,/|각각 더하기, 빼기, 곱하기, 나누기|
|%, MOD|나머지|
|IS|양쪽이 모두 TRUE 또는 FALSE|
|IS NOT|한쪽은 TRUE. 한쪽은 FALSE|
|AND, &&|양쪽이 모두 TRUE일 때만 TRUE|
|OR, ㅣㅣ|한쪽이 TRUE면 TRUE|
|=|양쪽 값이 같음|
|!=, <>|양쪽 값이 다름|
|>, <|(왼쪽, 오른쪽) 값이 더 큼|
|>=, <=|(왼쪽, 오른쪽)값이 같거나 더 큼|
|BETWEEN{MIN} AND {MAX}|두 값 사이에 있음|
|NOT BETWEEN{MIN} AND {MAX}|두 값 사이가 아닌 곳에 있음|
|IN(...)|괄호 안의 값들 가운데 있음|
|NOT IN(...)|괄호 안의 값들 가운데 없음|
|LIKE'...%...'|0~N개 문자를 가진 패턴|
|LIKE'..._....'|_갯수만큼의 문자를 가진 패턴|

## Lesson 3. 숫자와 문자열을 다루는 함수들

**1.숫자 관련 함수들**

|함수|설명|
|:---|:---|
|ROUND|반올림|
|CEIL|올림|
|FLOOR|내림|

```SQL
SELECT 
  ROUND(0.5),
  CEIL(0.4),
  FLOOR(0.6);
```
```SQL
SELECT 
  Price,
  ROUND(price),
  CEIL(price),
  FLOOR(price)
FROM Products;
```

|함수|설명|
|:---|:---|
|ABS|절댓값|

```SQL
SELECT ABS(1), ABS(-1), ABS(3 - 10);
```
```SQL
SELECT * FROM OrderDetails
WHERE ABS(Quantity - 10) < 5;
```

|함수|설명|
|:---|:---|
|GREATEST|(괄호 안에서)가장 큰 값|
|LEAST|(괄호 안에서) 가장 작은 값|
```SQL
SELECT 
  GREATEST(1, 2, 3),
  LEAST(1, 2, 3, 4, 5);
```
```SQL
SELECT
  OrderDetailID, ProductID, Quantity,
  GREATEST(OrderDetailID, ProductID, Quantity),
  LEAST(OrderDetailID, ProductID, Quantity)
FROM OrderDetails;
```

💡 그룹 함수 - 조건에 따라 집계된 값을 가져옵니다.
|함수|설명|
|:---|:---|
|MAX|가장 큰 값|
|MIN|가장 작은 값|
|COUNT|갯수 (NULL값 제외)|
|SUM|총합|
|AVG|평균 값|

```SQL
SELECT
  MAX(Quantity),
  MIN(Quantity),
  COUNT(Quantity),
  SUM(Quantity),
  AVG(Quantity)
FROM OrderDetails
WHERE OrderDetailID BETWEEN 20 AND 30;
```

|함수|설명|
|POW(A, B), POWER(A, B)|A를 B만틈 제곱|
|SQRT|제곱근|
```SQL
SELECT
  POW(2, 3),
  POWER(5, 2),
  SQRT(16);
```
```SQL
SELECT Price, POW(Price, 1/2)
FROM Products
WHERE SQRT(Price) < 4;
```

|함수|설명|
|:---|:---|
|TRUNCATE(N, n)|N을 소숫점 n자리까지 선택|
```SQL
SELECT
  TRUNCATE(1234.5678, 1),
  TRUNCATE(1234.5678, 2),
  TRUNCATE(1234.5678, 3),
  TRUNCATE(1234.5678, -1),
  TRUNCATE(1234.5678, -2),
  TRUNCATE(1234.5678, -3);
```
```SQL
SELECT Price FROM Products
WHERE TRUNCATE(Price, 0) = 12;
```

**2. 문자열 관련 함수들**

|함수|설명|
|:---|:---|
|UCASE, UPPER|모두 대문자로|
|LCASE, LOWER|모두 소문자|

```SQL
SELECT
  UPPER('abcDEF'),
  LOWER('abcDEF');
```
```SQL
SELECT
  UCASE(CustomerName),
  LCASE(ContactName)
FROM Customers;
```

|함수|설명|
|:---|:---|
|CONCAT(...)|괄호 안의 내용을 이어붙임|
|CONCAT_WS(S, ...)|괄호 안의 내용 S로 이어붙음|

```SQL
SELECT CONCAT('HELLO', ' ', 'THIS IS ', 2021)
```
```SQL
SELECT CONCAT_WS('-', 2021, 8, 15, 'AM')
```
```SQL
SELECT CONCAT('O-ID: ', OrderID) FROM Orders;
```
```SQL
SELECT
  CONCAT_WS(' ', FirstName, LastName) AS FullName
FROM Employees;
```

|함수|설명|
|:---|:---|
|SUBSTR, SUBSTRING|주어진 값에 따라 문자열 자름|
|LEFT|왼쪽부터 N글자|
|RIGHT|오른쪽부터 N글자|

```SQL
SELECT
  SUBSTR('ABCDEFG', 3),
  SUBSTR('ABCDEFG', 3, 2),
  SUBSTR('ABCDEFG', -4),
  SUBSTR('ABCDEFG', -4, 2);
```
![](https://cdn.discordapp.com/attachments/1102264938354978819/1135466394364952606/2023-07-31_144131.png)
```SQL
SELECT
  LEFT('ABCDEFG', 3),
  RIGHT('ABCDEFG', 3);
```
```SQL
SELECT
  OrderDate,
  LEFT(OrderDate, 4) AS Year,
  SUBSTR(OrderDate, 6, 2) AS Month,
  RIGHT(OrderDate, 2) AS Day
FROM Orders;
```

|함수|설명|
|:---|:---|
|LENGTH|문자열의 바이트 길이|
|CHAR_LENGTH, CHARACTER_LEGNTH|문자열의 문자 길이|

```SQL
SELECT
  LENGTH('ABCDE'),
  CHAR_LENGTH('ABCDE'),
  CHARACTER_LENGTH('ABCDE');
```
```SQL
-- w3wchool 사이트에서는 한글이 제대로 동작하지 않습니다.
SELECT
  LENGTH('안녕하세요'), -- 15
  CHAR_LENGTH('안녕하세요'), -- 5
  CHARACTER_LENGTH('안녕하세요'); -- 5
```

|함수|설명|
|:---|:---|
|TRIM|양쪽 공백 제거|
|LTRIM|왼쪽 공백 제거|
|RTRIM|오른쪽 공백 제거|

```SQL
SELECT
  CONCAT('|', ' HELLO ', '|'),
  CONCAT('|', LTRIM(' HELLO '), '|'),
  CONCAT('|', RTRIM(' HELLO '), '|'),
  CONCAT('|', TRIM(' HELLO '), '|');
```
```SQL
SELECT * FROM Categories
WHERE CategoryName = ' Beverages '
```
```SQL
SELECT * FROM Categories
WHERE CategoryName = TRIM(' Beverages ')
```

|함수|설명|
|:---|:---|
|LPAD(S, N, P)|S가 N글자가 될 때까지 P를 이어붙임|
|RPAD(S, N, P)|S가 N글자가 될 때까지 P를 이어붙임|

```SQL
SELECT
  LPAD('ABC', 5, '-'),
  RPAD('ABC', 5, '-');
```
```SQL
SELECT
  LPAD(SupplierID, 5, 0),
  RPAD(Price, 6, 0)
FROM Products;
```

|함수|설명|
|:---|:---|
|REPLACE(S, A, B)|S중 A를 B로 변경|
```SQL
SELECT
  REPLACE('맥도날드에서 맥도날드 햄버거를 먹었다.', '맥도날드', '버거킹');
```
```SQL
SELECT
  REPLACE(Description, ', ', ' and ')
FROM Categories;
```

|함수|설명|
|:---|:---|
|INSTR(S, s)|S중 s의 첫 위치 반환, 앖을 시 0|
```SQL
SELECT
  INSTR('ABCDE', 'ABC'),
  INSTR('ABCDE', 'BCDE'),
  INSTR('ABCDE', 'C'),
  INSTR('ABCDE', 'DE'),
  INSTR('ABCDE', 'F');
```
```SQL
SELECT * FROM Customers
WHERE INSTR(CustomerName, ' ') BETWEEN 1 AND 6;
-- < 6으로 하면?
```

|함수|설명|
|CAST(A AS T)|A를 T 자료형으로 변환|
|CONVERT(A, T)|A를 T 자료형으로 변환|

```SQL
SELECT
  '01' = '1',
  CAST('01' AS DECIMAL) = CAST('1' AS DECIMAL);
```
```SQL
SELECT
  '01' = '1',
  CONVERT('01', DECIMAL) = CONVERT('1', DECIMAL);
```

## Lesson 4. 시간/날짜 관련 및 기타 함수들

#### 1. 시간/날짜 관련 함수들
|함수|설명|
|:---|:---|
|CURRENT_DATE, CURDATE|현재 날짜 반환|
|CURRENT_TIME, CURTIM|현재 시간 반환|
|CURRENT_TIMETAMP, NOW|현재 시간과 날짜 반환|
```SQL
SELECT CURDATE(), CURTIME(), NOW();
```

|함수|설명|
|:---|:---|
|DATE|문자열에 따라 날짜 생성|
|TIME|문자열에 따라 시간 생성|

```SQL
SELECT
  '2021-6-1' = '2021-06-01',
  DATE('2021-6-1') = DATE('2021-06-01'),
  '1:2:3' = '01:02:03',
  TIME('1:2:3') = TIME('01:02:03');
```
```SQL
SELECT
  '2021-6-1 1:2:3' = '2021-06-01 01:02:03',
  DATE('2021-6-1 1:2:3') = DATE('2021-06-01 01:02:03'),
  TIME('2021-6-1 1:2:3') = TIME('2021-06-01 01:02:03'),
  DATE('2021-6-1 1:2:3') = TIME('2021-06-01 01:02:03'),
  DATE('2021-6-1') = DATE('2021-06-01 01:02:03'),
  TIME('2021-6-1 1:2:3') = TIME('01:02:03');
```
```SQL
SELECT * FROM Orders
WHERE
  OrderDate BETWEEN DATE('1997-1-1') AND DATE('1997-1-31');
```

|함수|설명|
|:---|:---|
|YEAR|주어진 DATETIME값의 년도 반환|
|MONTHNAME|주어진 DATETIME값의 월(영문) 반환|
|MONTH|주어진 DATETIME값의 월 반환|
|WEEKDAY|주어진 DATETIME값의 요일값 반환(월요일: 0)|
|DAYNAME|주언진 DATETIME값의 요일명 반환|
|DAYOFMONTH, DAY|주어진 DATETIME값의 날짜(일) 반환|

```SQL
SELECT
  OrderDate,
  YEAR(OrderDate) AS YEAR,
  MONTHNAME(OrderDate) AS MONTHNAME,
  MONTH(OrderDate) AS MONTH,
  WEEKDAY(OrderDate) AS WEEKDAY,
  DAYNAME(OrderDate) AS DAYNAME,
  DAY(OrderDate) AS DAY
FROM Orders;
```
```SQL
SELECT
  OrderDate,
  CONCAT(
    CONCAT_WS(
      '/',
      YEAR(OrderDate), MONTH(OrderDate), DAY(OrderDate)
    ),
    ' ',
    UPPER(LEFT(DAYNAME(OrderDate), 3))
  )
FROM Orders;
```
```SQL
SELECT * FROM Orders
WHERE WEEKDAY(OrderDate) = 0;
```

|함수|설명|
|:---|:---|
|HOUR|주어진 DATETIME의 시 반환|
|MINUTE|주어진 DATETIME의 분 반환|
|SECOND|주어진 DATETIME의 초 반환|

```SQL
SELECT
  HOUR(NOW()), MINUTE(NOW()), SECOND(NOW());
```

|함수|설명|
|:---|:---|
|ADDDATE, DATE_ADD|시간/날짜 더하기|
|SURDATE, DATE_SUB|

```SQL
SELECT 
  ADDDATE('2021-06-20', INTERVAL 1 YEAR),
  ADDDATE('2021-06-20', INTERVAL -2 MONTH),
  ADDDATE('2021-06-20', INTERVAL 3 WEEK),
  ADDDATE('2021-06-20', INTERVAL -4 DAY),
  ADDDATE('2021-06-20', INTERVAL -5 MINUTE),
  ADDDATE('2021-06-20 13:01:12', INTERVAL 6 SECOND);
```
```SQL
SELECT
  OrderDate,
  ADDDATE(OrderDate, INTERVAL 1 YEAR),
  ADDDATE(OrderDate, INTERVAL -2 MONTH),
  ADDDATE(OrderDate, INTERVAL 3 WEEK),
  ADDDATE(OrderDate, INTERVAL -4 DAY),
  ADDDATE(OrderDate, INTERVAL -5 MINUTE)
FROM Orders;
```

|함수|설명|
|:---|:---|
|DATE_DIFF|두 시간/날짜 간 일수차|
|TIME_DIFF|두 시간/날짜 간 시간차|

```SQL
SELECT
  OrderDate,
  NOW(),
  DATEDIFF(OrderDate, NOW())
FROM Orders;
```
```SQL
SELECT
  TIMEDIFF('2021-06-21 15:20:35', '2021-06-21 16:34:41');
```
```SQL
SELECT * FROM Orders
WHERE
  ABS(DATEDIFF(OrderDate, '1996-10-10')) < 5;
```

|함수|설명|
|:---|:---|
|LAST_DAY|해당 달의 마지막 날짜|

```SQL
SELECT
  OrderDate,
  LAST_DAY(OrderDate),
  DAY(LAST_DAY(OrderDate)),
  DATEDIFF(LAST_DAY(OrderDate), OrderDate)
FROM Orders;
```

|함수|설명|
|:---|:---|
|DATE_FORMAT|시간/날짜를 지정한 형식으로 반환|

|함수|설명|
|:---|:---|
|%Y|년도 4자리|
|%y|년도 2자리|
|%M|월 영문|
|%m|월 숫자|
|%D|일 영문(1st, 2nd, 3rd...)|
|%d, %e|일 숫자(01 ~ 31)|
|%T|hh:mm:ss|
|%r|hh:mm:ss AM/PM|
|%H, %K|시 (~23)|
|%h, %l|시 (~12)|
|%i|분|
|%S, %s|초|
|%p|AM/PM|

```SQL
SELECT
  DATE_FORMAT(NOW(), '%M %D, %Y %T'),
  DATE_FORMAT(NOW(), '%y-%m-%d %h:%i:%s %p'),
  DATE_FORMAT(NOW(), '%Y년 %m월 %d일 %p %h시 %i분 %s초');
```
```SQL
SELECT REPLACE(
  REPLACE(
    DATE_FORMAT(NOW(), '%Y년 %m월 %d일 %p %h시 %i분 %초'),
    'AM', '오전'
  ),
  'PM', '오후'
)
```

|함수|설명|
|:---|:---|
|STR_TO_DATE(S, F)|S를 F형식으로 해석하여 시간/날짜 생성|

```SQL
SELECT
  DATEDIFF(
    STR_TO_DATE('2021-06-04 07:48:52', '%Y-%m-%d %T'),
    STR_TO_DATE('2021-06-01 12:30:05', '%Y-%m-%d %T')
  ),
  TIMEDIFF(
    STR_TO_DATE('2021-06-04 07:48:52', '%Y-%m-%d %T'),
    STR_TO_DATE('2021-06-01 12:30:05', '%Y-%m-%d %T')
  );
```
```SQL
SELECT
  OrderDate,
  DATEDIFF(
    STR_TO_DATE('1997-01-01 13:24:35', '%Y-%m-%d %T'),
    OrderDate
  ),
  TIMEDIFF(
    STR_TO_DATE('1997-01-01 13:24:35', '%Y-%m-%d %T'),
    STR_TO_DATE(CONCAT(OrderDate, ' ', '00:00:00'), '%Y-%m-%d %T')
  )
FROM Orders;
```

#### 2.기타 함수들

|형식|설명|
|:---|:---|
|IF(조건, T, F)|조건이 참이라면 T, 거짓이면 F 반환|

```SQL
SELECT IF (1 > 2, '1은 2보다 크다.', '1은 2보다 작다.');
```
💡 보다 복잡한 조건은 CASE문을 사용합니다.
```SQL
SELECT
CASE
  WHEN -1 > 0 THEN '-1은 양수다.'
  WHEN -1 = 0 THEN '-1은 0이다.'
  ELSE '-1은 음수다.'
END;
```
```SQL
SELECT
  Price,
  IF (Price > 30, 'Expensive', 'Cheap'),
  CASE
    WHEN Price < 20 THEN '저가'
    WHEN Price BETWEEN 20 AND 30 THEN '일반'
    ELSE '고가'
  END
FROM Products;
```

|형식|설명|
|:---|:---|
|IFNULL(A, B)|A가 NULL일 시 B 출력|

```SQL
SELECT
  IFNULL('A', 'B'),
  IFNULL(NULL, 'B');
```
## LESSON 5. 조건에 따라 그룹으로 묶기

**1-3 복습 - 💡 그룹 함수**

⭐ NULL값은 집계하지 않습니다.

|함수|설명|
|:---|:---|
|MAX|가장 큰 값|
|MIN|가장 작은 값|
|COUNT|갯수(NULL값 제외)|
|SUM|총합|
|AVG|평균값|

#### 1. GROUP BY - 조건에 따라 집계된 값을 가져옵니다.

```SQL
SELECT Country FROM Customers
GROUP BY Country;
```
```SQL
SELECT CategoryID FROM Products
GROUP BY CategoryID;
```
💡 여러 컬럼을 기준으로 그룹화할 수도 있습니다.

```SQL
SELECT 
  Country, City,
  CONCAT_WS(', ', City, Country)
FROM Customers
GROUP BY Country, City;
```
**📚 그룹 함수 활용하기**

```SQL
SELECT
  COUNT(*), OrderDate
FROM Orders
GROUP BY OrderDate;
```
```SQL
SELECT
  ProductID,
  SUM(Quantity) AS QuantitySum
FROM OrderDetails
GROUP BY ProductID
ORDER BY QuantitySum DESC;
```
```SQL
SELECT
  CategoryID,
  MAX(Price) AS MaxPrice, 
  MIN(Price) AS MinPrice,
  TRUNCATE((MAX(Price) + MIN(Price)) / 2, 2) AS MedianPrice,
  TRUNCATE(AVG(Price), 2) AS AveragePrice
FROM Products
GROUP BY CategoryID;
```
```SQL
SELECT 
  CONCAT_WS(', ', City, Country) AS Location,
  COUNT(CustomerID)
FROM Customers
GROUP BY Country, City;
```

**💡 WITH ROLLUP - 전체의 집계값**

```SQL
SELECT
  Country, COUNT(*)
FROM Suppliers
GROUP BY Country
WITH ROLLUP;
```

- 위의 각 집계함수 쿼리 끝에 WITH ROLLUP 을 추가해보세요.
- ⚠️ ORDER BY 와는 함께 사용될 수 없습니다.

**HAVING - 그룹화된 데이터 걸러내기**

```SQL
SELECT
  Country, COUNT(*) AS Count
FROM Suppliers
GROUP BY Country
HAVING Count >= 3;
```

**💡 WHERE는 그룹하기 전 데이터, HAVING은 그룹 후 집계에 사용합니다.**

```SQL
SELECT
  COUNT(*) AS Count, OrderDate
FROM Orders
WHERE OrderDate > DATE('1996-12-31')
GROUP BY OrderDate
HAVING Count > 2;
```
```SQL
SELECT
  CategoryID,
  MAX(Price) AS MaxPrice, 
  MIN(Price) AS MinPrice,
  TRUNCATE((MAX(Price) + MIN(Price)) / 2, 2) AS MedianPrice,
  TRUNCATE(AVG(Price), 2) AS AveragePrice
FROM Products
WHERE CategoryID > 2
GROUP BY CategoryID
HAVING
  AveragePrice BETWEEN 20 AND 30
  AND MedianPrice < 40;
```

#### 2. DISTINCT - 중복된 값들을 제거합니다.

- GROUP BY와 달리 집계함수가 사용되지 않습니다.
- GROUP BY와 달리 정렬하지 않으므로 더 빠릅니다.

```SQL
SELECT DISTINCT CategoryID
FROM Products;
-- 위의 GROUP BY를 사용한 쿼리와 결과 비교
```
```SQL
SELECT COUNT DISTINCT CategoryID
FROM Products;
-- 오류 발생
```
```SQL
SELECT DISTINCT Country
FROM Customers
ORDER BY Country;
```
```SQL
SELECT DISTINCT Country, City
FROM Customers
ORDER BY Country, City;
```

**💡 GROUP BY와 DISTINCT 함께 활용하기**

```SQL
SELECT
  Country,
  COUNT(DISTINCT CITY)
FROM Customers
GROUP BY Country;
```