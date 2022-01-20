# Shopify Data Science Challenge
Within lies my answers to the Shopify Data Science Challenge.

### Question 1
The answer to question one consists of a [Jupyter notebook](question1.ipynb) with a series of annotations. The provided dataset for this question is uploaded and renamed here as `data_sneakers.csv`. Please run the notebook and read the annotations to get the answers.

### Question 2
The answers and queries for question 2 are in the text file [here](question2.txt) and are listed below Queries can be tested at at [this link](https://www.w3schools.com/SQL/TRYSQL.ASP?FILENAME=TRYSQL_SELECT_ALL). Note that these queries are not the most optimised. They use `GROUP BY` where I might not have to since I'm a little rusty with SQL. So I tried to write the queries to be more legible than they are optimised.

#### Q1.1
```MySQL
SELECT COUNT(Orders.ShipperID) 
FROM Shippers
INNER JOIN Orders ON Orders.ShipperID = Shippers.ShipperID
WHERE Shippers.ShipperName = "Speedy Express"
```
output: `54`

#### Q1.2
```MySQL
SELECT Employees.LastName
FROM Orders
INNER JOIN Employees ON Employees.EmployeeID = Orders.EmployeeID
GROUP BY Orders.EmployeeID
ORDER BY COUNT(Orders.EmployeeID) DESC LIMIT  1
```
ouput: `Peacock`

#### Q1.3
```MySQL
SELECT Products.ProductName as pname
FROM  Products
INNER JOIN Customers ON Customers.CustomerID = Orders.CustomerID
INNER JOIN Orders ON OrderDetails.OrderID = Orders.OrderID
INNER JOIN OrderDetails ON Products.ProductID = OrderDetails.ProductID
WHERE Customers.country = "Germany"
GROUP BY pname
ORDER BY COUNT(Products.ProductName) DESC LIMIT 1
```
output: `Gorgonzola Telino`
