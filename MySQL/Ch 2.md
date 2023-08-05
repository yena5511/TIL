## Lesson 1. 쿼리 안에 서브쿼리

#### 1. 비상관 서브쿼리

```SQL
SELECT
  CategoryID, CategoryName, Description,
  (SELECT ProductName FROM Products WHERE ProductID = 1)
FROM Categories;
```
```SQL
SELECT * FROM Products
WHERE Price < (
  SELECT AVG(Price) FROM Products
);
```
```SQL
SELECT
  CategoryID, CategoryName, Description
FROM Categories
WHERE
  CategoryID =
  (SELECT CategoryID FROM Products
  WHERE ProductName = 'Chais');
```
```SQL
SELECT
  CategoryID, CategoryName, Description
FROM Categories
WHERE
  CategoryID IN
  (SELECT CategoryID FROM Products
  WHERE Price > 50);
```
|함수|의미|
|:---|:---|
|~ALL|서브쿼리의 모든 결과에 대해 ~하다|
|~ANY|서브쿼리의 하나 이상의 결과에 대해 ~하다|

```SQL
SELECT * FROM Products
WHERE Price > ALL (
  SELECT Price FROM Products
  WHERE CategoryID = 2
);
```
```SQL
SELECT
  CategoryID, CategoryName, Description
FROM Categories
WHERE
  CategoryID = ANY
  (SELECT CategoryID FROM Products
  WHERE Price > 50);
```

#### 2. 상관 서브쿼리

```SQL
SELECT
  ProductID, ProductName,
  (
    SELECT CategoryName FROM Categories C
    WHERE C.CategoryID = P.CategoryID
  ) AS CategoryName
FROM Products P;
```
```SQL
SELECT
  SupplierName, Country, City,
  (
    SELECT COUNT(*) FROM Customers C
    WHERE C.Country = S.Country
  ) AS CustomersInTheCountry,
  (
    SELECT COUNT(*) FROM Customers C
    WHERE C.Country = S.Country 
      AND C.City = S.City
  ) AS CustomersInTheCity
FROM Suppliers S;
```
```SQL
SELECT
  CategoryID, CategoryName,
  (
    SELECT MAX(Price) FROM Products P
    WHERE P.CategoryID = C.CategoryID
  ) AS MaximumPrice,
  (
    SELECT AVG(Price) FROM Products P
    WHERE P.CategoryID = C.CategoryID
  ) AS AveragePrice
FROM Categories C;
```
```SQL
SELECT
  ProductID, ProductName, CategoryID, Price
  -- ,(SELECT AVG(Price) FROM Products P2
  -- WHERE P2.CategoryID = P1.CategoryID)
FROM Products P1
WHERE Price < (
  SELECT AVG(Price) FROM Products P2
  WHERE P2.CategoryID = P1.CategoryID
);
```

**EXISTS / NOT EXISTS 연산자**

```SQL
SELECT
  CategoryID, CategoryName
  -- ,(SELECT MAX(P.Price) FROM Products P
  -- WHERE P.CategoryID = C.CategoryID
  -- ) AS MaxPrice
FROM Categories C
WHERE EXISTS (
  SELECT * FROM Products P
  WHERE P.CategoryID = C.CategoryID
  AND P.Price > 80
);
```

## Lesson 2. JOIN - 여러 데이블 조립하기


#### 1. JOIN(INNER JOIN) - 내부 조인

- 양쪽 모두에 값이 있는 행(NOT NULL) 반환
- 'INNER'는 선택사항


```SQL
SELECT * FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID; 
```
```SQL
SELECT * FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID; 
```
```SQL
SELECT * FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID; 
```

**💡 여러 테이블을 JOIN할 수 있습니다**

```SQL
SELECT 
  C.CategoryID, C.CategoryName, 
  P.ProductName, 
  O.OrderDate,
  D.Quantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID;
```

**💡 JOIN한 테이블 GROUP하기**

```SQL
SELECT 
  C.CategoryName,
  MIN(O.OrderDate) AS FirstOrder,
  MAX(O.OrderDate) AS LastOrder,
  SUM(D.Quantity) AS TotalQuantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID
GROUP BY C.CategoryID;
```
```SQL
SELECT 
  C.CategoryName, P.ProductName,
  MIN(O.OrderDate) AS FirstOrder,
  MAX(O.OrderDate) AS LastOrder,
  SUM(D.Quantity) AS TotalQuantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID
GROUP BY C.CategoryID, P.ProductID;
```

**💡 SELF JOIN - 같은 테이블끼리**

```SQL
SELECT
  E1.EmployeeID, CONCAT_WS(' ', E1.FirstName, E1.LastName) AS Employee,
  E2.EmployeeID, CONCAT_WS(' ', E2.FirstName, E2.LastName) AS NextEmployee
FROM Employees E1 JOIN Employees E2
ON E1.EmployeeID + 1 = E2.EmployeeID;

-- 1번의 전, 마지막 번호의 다음은?
```

##### 2. LEFT/RIGHT OUTER JOIN - 외부 조인

- 반대쪽에 데이터가 있든 없든(NULL), 선택된 방향에 있으면 출력 - 행 수 결정
- 'OTHER'는 선택사햘

```SQL
SELECT
  E1.EmployeeID, CONCAT_WS(' ', E1.FirstName, E1.LastName) AS Employee,
  E2.EmployeeID, CONCAT_WS(' ', E2.FirstName, E2.LastName) AS NextEmployee
FROM Employees E1
LEFT JOIN Employees E2
ON E1.EmployeeID + 1 = E2.EmployeeID
ORDER BY E1.EmployeeID;

-- LEFT를 RIGHT로 바꿔서도 실행해 볼 것
```

```SQL
SELECT
  IFNULL(C.CustomerName, '-- NO CUSTOMER --'),
  IFNULL(S.SupplierName, '-- NO SUPPLIER --'),
  IFNULL(C.City, S.City),
  IFNULL(C.Country, S.Country)
FROM Customers C
LEFT JOIN Suppliers S
ON C.City = S.City AND C.Country = S.Country;

-- LEFT를 RIGHT로 바꿔서도 실행해 볼 것
```

```SQL
SELECT
  IFNULL(C.CustomerName, '-- NO CUSTOMER --'),
  IFNULL(S.SupplierName, '-- NO SUPPLIER --'),
  IFNULL(C.City, S.City),
  IFNULL(C.Country, S.Country)
FROM Customers C
LEFT JOIN Suppliers S
ON C.City = S.City AND C.Country = S.Country;

-- LEFT를 RIGHT로 바꿔서도 실행해 볼 것
```

#### CROSS JOIN - 교차 조인
- 조건 없이 모든 조합 반환(A*B)
```SQL
SELECT
  E1.LastName, E2.FirstName
FROM Employees E1
CROSS JOIN Employees E2
ORDER BY E1.EmployeeID;
```

## Lesson 3. UNION - 집합으로 다루기


|연산자|설명|
|:---|:---|
|UNION|중복을 제거한 집합|
|UNION ALL|중복을 제거하지 않은 집합|

```SQL
SELECT CustomerName AS Name, City, Country, 'CUSTOMER'
FROM Customers
UNION
SELECT SupplierName AS Name, City, Country, 'SUPPLIER'
FROM Suppliers
ORDER BY Name;
```

**합집합**
![](https://www.yalco.kr/images/lectures/sql/2-3/01.png)

```SQL
SELECT CategoryID AS ID FROM Categories
WHERE CategoryID > 4
UNION
SELECT EmployeeID AS ID FROM Employees
WHERE EmployeeID % 2 = 0;

-- UNION ALL로 바꿔볼 것
```

**교집합**
![](https://www.yalco.kr/images/lectures/sql/2-3/02.png)

```SQL
SELECT CategoryID AS ID
FROM Categories C, Employees E
WHERE 
  C.CategoryID > 4
  AND E.EmployeeID % 2 = 0
  AND C.CategoryID = E.EmployeeID;
```

**차집합**
![](https://www.yalco.kr/images/lectures/sql/2-3/03.png)

```SQL
SELECT CategoryID AS ID
FROM Categories
WHERE 
  CategoryID > 4
  AND CategoryID NOT IN (
    SELECT EmployeeID
    FROM Employees
    WHERE EmployeeID % 2 = 0
  );
```

**대칭차집합**

![](https://www.yalco.kr/images/lectures/sql/2-3/04.png)

```SQL
SELECT ID FROM (
  SELECT CategoryID AS ID FROM Categories
  WHERE CategoryID > 4
  UNION ALL
  SELECT EmployeeID AS ID FROM Employees
  WHERE EmployeeID % 2 = 0
) AS Temp 
GROUP BY ID HAVING COUNT(*) = 1;
```
