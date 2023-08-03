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