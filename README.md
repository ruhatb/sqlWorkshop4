# sqlWorkshop4


--En Pahalı Ürünü Getirin--
SELECT *FROM Products
ORDER BY Price DESC
LIMIT 1;

--En Son Verilen Siparişi Bulun--
SELECT *FROM Orders
ORDER BY OrderDate DESC
LIMIT 1;

--Fiyatı Ortalama Fiyattan Yüksek Olan Ürünleri Getirin--
SELECT *FROM Products
WHERE Price > (SELECT AVG(Price) FROM Products);

--Belirli Kategorilerdeki Ürünleri Listeleyin--
SELECT *FROM Products
WHERE CategoryID IN (1, 2, 3);  

--En Yüksek Fiyatlı Ürünlere Sahip Kategorileri Listeleyin--
SELECT CategoryID, MAX(Price) as MaxPrice FROM Products
GROUP BY CategoryID
ORDER BY MaxPrice DESC;

--Bir Ülkedeki Müşterilerin Verdiği Siparişleri Listeleyin--
SELECT *FROM Orders
WHERE CustomerID IN (SELECT CustomerID FROM Customers WHERE Country = 'Turkey');  

--Her Kategori İçin Ortalama Fiyatın Üzerinde Olan Ürünleri Listeleyin--
SELECT *FROM Products p
WHERE Price > (SELECT AVG(Price) FROM Products WHERE CategoryID = p.CategoryID);

--Her Müşterinin En Son Verdiği Siparişi Listeleyin--
SELECT o*FROM Orders o
JOIN (SELECT CustomerID, MAX(OrderDate) as LastOrderDate
      FROM Orders
      GROUP BY CustomerID) last_orders
ON o.CustomerID = last_orders.CustomerID AND o.OrderDate = last_orders.LastOrderDate;

--Her Çalışanın Kendi Departmanındaki Ortalama Maaşın Üzerinde Maaş Alıp Almadığını Bulun--
SELECT * (SELECT AVG(Salary)  FROM Employees 
        WHERE DepartmentID = e.DepartmentID) as AvgSalary
FROM Employees e
WHERE e.Salary > (SELECT AVG(Salary)  FROM Employees 
WHERE DepartmentID = e.DepartmentID);

--En Az 10 Ürün Satın Alınan Siparişleri Listeleyin--
SELECT OrderID, COUNT(ProductID) as ProductCount FROM OrderDetails
GROUP BY OrderID
HAVING ProductCount >= 10;

-- Her Kategoride En Pahalı Olan Ürünlerin Ortalama Fiyatını Bulun--
SELECT AVG(Price) as AvgMaxPrice FROM (SELECT CategoryID, MAX(Price) as MaxPrice
FROM Products
GROUP BY CategoryID) as MaxPrices;

--Müşterilerin Verdiği Toplam Sipariş Sayısına Göre Sıralama Yapın--
SELECT CustomerID, COUNT(OrderID) as Total FROM Orders
GROUP BY CustomerID
ORDER BY Total DESC;

-- En Fazla Sipariş Vermiş 5 Müşteriyi ve Son Sipariş Tarihlerini Listeleyin--
SELECT CustomerID, COUNT(OrderID) as TotalOrders, MAX(OrderDate) as LastOrderDate FROM Orders
GROUP BY CustomerID
ORDER BY TotalOrders DESC
LIMIT 5;


--Toplam Ürün Sayısı 15'ten Fazla Olan Kategorileri Listeleyin--
SELECT CategoryID FROM Products
GROUP BY CategoryID
HAVING COUNT(ProductID) > 15;

--En Fazla 5 Farklı Ürün Sipariş Eden Müşterileri Listeleyin--
SELECT CustomerID FROM Orders
JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
GROUP BY CustomerID
HAVING COUNT(DISTINCT ProductID) <= 5;

--20'den Fazla Ürün Sağlayan Tedarikçileri Listeleyin--
SELECT SupplierID FROM Products
GROUP BY SupplierID
HAVING COUNT(ProductID) > 20;

--Her Müşteri İçin En Pahalı Ürünü Bulun--
SELECT CustomerID, MAX(Price) as MaxPrice FROM Orders o
JOIN OrderDetails od ON o.OrderID = od.OrderID
JOIN Products p ON od.ProductID = p.ProductID
GROUP BY CustomerID;

--10.000'den Fazla Sipariş Değeri Olan Çalışanları Listeleyin--
SELECT EmployeeID FROM Orders
GROUP BY EmployeeID
HAVING SUM(TotalAmount) > 10000;

-- Kategorisine Göre En Çok Sipariş Edilen Ürünü Bulun--
SELECT ProductID, COUNT(OrderID) as Count FROM OrderDetails
GROUP BY ProductID
ORDER BY Count DESC
LIMIT 1;

--Müşterilerin En Son Sipariş Verdiği Ürün ve Tarihlerini Listeleyin--
SELECT o.CustomerID, od.ProductID, o.OrderDate FROM Orders o
JOIN OrderDetails od ON o.OrderID = od.OrderID
WHERE o.OrderDate = (SELECT MAX(OrderDate) FROM Orders WHERE CustomerID = o.CustomerID);


--Her Çalışanın Teslim Ettiği En Pahalı Siparişi ve Tarihini Listeleyin--
SELECT o.EmployeeID, MAX(TotalAmount) as MaxOrder, o.OrderDate FROM Orders o
GROUP BY o.EmployeeID;

--En Fazla Sipariş Verilen Ürünü ve Bilgilerini Listeleyin--
SELECT od.ProductID, COUNT(od.OrderID) as Count FROM OrderDetails 
GROUP BY orderDetails.ProductID
ORDER BY Count DESC
LIMIT 1;
