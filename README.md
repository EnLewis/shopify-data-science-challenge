# Shopify Data Science Challenge Fall 2022
## Question 1
- Q1.a) **Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.**<p>
       - At first glance, I imagine that some high-end outliers in our dataset are skewing the AOV.<br>
- Q1.b) **What metric would you report for this dataset?**<p>
       - In that case the mean should work a lot better for us in approximating AOV.<br>
- Q1.c) **What is its value?**<p>
       - The mean order amount is 284.00<br>
- Bonus)
        - `shop_id:42` and `shop_id:78` are either glitched or don't belong in this dataset. Removing both those 2 shops from our dataset drops 97 rows (only 1.9% of our total dataset) and cleans up our metrics significantly. After that clean up the mean shows `300.16` without any extra trimming needed.

The detailed reasoning behind my answers to question 1 can be found in a [Jupyter notebook](question1.ipynb) with a series of annotations. The provided dataset for this question is uploaded and renamed here as `data_sneakers.csv`. Please run the notebook and read the annotations to get the answers.

## Question 2
The answers and queries for question 2 are in the text file [here](question2.txt) and are listed below Queries can be tested at at [this link](https://www.w3schools.com/SQL/TRYSQL.ASP?FILENAME=TRYSQL_SELECT_ALL). I have made the assumption that whatever SQL engine I'm using optimizes better with JOIN rather than sub-queries. With that assumption, I've opted to minimize sub-queries except for at the end for selecting a final answer.

### Q1.a
**How many orders were shipped by Speedy Express in total?**
```MySQL
SELECT Count(*) as SpeedyExpress_OrderCount
FROM   Shippers as shp
       INNER JOIN Orders O
               ON shp.ShipperID = O.ShipperID
WHERE  shp.ShipperName = "Speedy Express"
```
| **SpeedyExpress_OrderCount**  |
|---|
|  54 |

### Q1.b
**What is the last name of the employee with the most orders?**
```MySQL
SELECT EmployeeOfTheMonth
FROM   (SELECT LastName as EmployeeOfTheMonth, Max(count) as TopNumberSales
        FROM   (SELECT Employees.LastName, Count(Orders.EmployeeID) as count
                FROM   Orders
                       JOIN Employees
                         ON Employees.EmployeeID = Orders.EmployeeID
                GROUP  BY Orders.EmployeeID))  
```
| **EmployeeOfTheMonth**  |
|---|
|  Peacock |

### Q1.c
**What product was ordered the most by customers in Germany?**<br>
NOTE: I'm assuming this is asking for total quantity of product ordered, not total number of orders placed for a product.<br>
i.e: If there is 1 order of 100 of product A, and 10 orders of 1 of product B; the query should output product A as the answer. 
```MySQL
SELECT ProductName as GermanysFavouriteProduct
FROM   (SELECT ProductName, Max(total_quant) AS TotalProductQuantity
        FROM   (SELECT Products.ProductName, Sum(Quantity) AS total_quant
                FROM   Products
                       INNER JOIN Customers
                               ON Customers.CustomerID = Orders.CustomerID
                       INNER JOIN Orders
                               ON OrderDetails.OrderID = Orders.OrderID
                       INNER JOIN OrderDetails
                               ON Products.ProductID = OrderDetails.ProductID
                WHERE  Customers.country = "Germany"
                GROUP  BY Products.ProductID)) 
```
| **GermanysFavouriteProduct**  |
|---|
|  Boston Crab Meat |
