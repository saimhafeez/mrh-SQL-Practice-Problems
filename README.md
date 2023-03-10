# 57 SQL Practice Problem Solutions

# Introductory Problems

## 1. Which shippers do we have?

We have a table called Shippers. Return all the fields from all the shippers

```sql
SELECT * FROM Shippers
```

## 2. Certain fields from Categories

In the Categories table, selecting all the fields using this SQL:

**Select * from Categories**

…will return 4 columns. We only want to see two columns, CategoryName and Description.

```sql
SELECT CategoryName, Description FROM Categories
```

## 3. Sales Representatives

We’d like to see just the FirstName, LastName, and HireDate of all the employees with the Title of Sales Representative. Write a SQL statement that returns only those employees.

```sql
SELECT FirstName, LastName, HireDate FROM Employees WHERE Title = 'Sales Representative'
```

## 4. Sales Representatives in the United States

Now we’d like to see the same columns as above, but only for those employees that both have the title of Sales Representative, and also are in the United States.

```sql
SELECT FirstName, LastName, HireDate FROM Employees WHERE Title = 'Sales Representative' AND Country = 'USA'
```

## 5. Orders placed by specific EmployeeID

Show all the orders placed by a specific employee. The EmployeeID for this Employee (Steven Buchanan) is 5.

```sql
SELECT * FROM Orders WHERE EmployeeID = 5
```

## 6. Suppliers and ContactTitles

In the Suppliers table, show the SupplierID, ContactName, and ContactTitle for those Suppliers whose ContactTitle is not Marketing Manager.

```sql
SELECT SupplierID, ContactName, ContactTitle FROM Suppliers WHERE NOT ContactTitle = 'Marketing Manager'
```

## 7. Products with “queso” in ProductName

In the products table, we’d like to see the ProductID and ProductName for those products where the ProductName includes the string “queso”.

```sql
SELECT ProductID, ProductName FROM Products WHERE ProductName LIKE '%queso%'
```

## 8. Orders shipping to France or Belgium

Looking at the Orders table, there’s a field called ShipCountry. Write a query that shows the OrderID, CustomerID, and ShipCountry for the orders where the ShipCountry is either France or Belgium.

```sql
SELECT OrderID, CustomerID, ShipCountry FROM Orders WHERE ShipCountry = 'France' OR ShipCountry = 'Belgium'
```

## 9. Orders shipping to any country in Latin America

Now, instead of just wanting to return all the orders from France of Belgium, we want to show all the orders from any Latin American country. But we don’t have a list of Latin American countries in a table in the Northwind database. So, we’re going to just use this list of Latin

American countries that happen to be in the Orders table: Brazil Mexico Argentina Venezuela It doesn’t make sense to use multiple Or statements anymore, it would get too convoluted. Use the In statement.

```sql
SELECT OrderID, CustomerID, ShipCountry FROM Orders WHERE ShipCountry IN ('Brazil', 'Mexico', 'Argentina', 'Venezuela')
```

## 10. Employees, in order of age

For all the employees in the Employees table, show the FirstName, LastName, Title, and BirthDate. Order the results by BirthDate, so we have the oldest employees first.

```sql
SELECT FirstName, LastName, Title, BirthDate FROM Employees ORDER BY BirthDate ASC
```

## 11. Showing only the Date with a DateTime field

In the output of the query above, showing the Employees in order of BirthDate, we see the time of the BirthDate field, which we don’t want. Show only the date portion of the BirthDate field.

```sql
SELECT FirstName, LastName, Title, BirthDate = CONVERT(date, BirthDate) FROM Employees ORDER BY BirthDate ASC
```

## 12. Employees full name

Show the FirstName and LastName columns from the Employees table, and then create a new column called FullName, showing FirstName and LastName joined together in one column, with a space in-between.

```sql
SELECT FirstName, LastName, FullName = FirstName + ' ' + LastName FROM Employees
```

## 13. OrderDetails amount per line item

In the OrderDetails table, we have the fields UnitPrice and Quantity. Create a new field, TotalPrice, that multiplies these two together. We’ll ignore the Discount

field for now.

In addition, show the OrderID, ProductID, UnitPrice, and Quantity. Order by OrderID and ProductID.

```sql
SELECT OrderID, ProductID, UnitPrice, Quantity, TotalPrice = UnitPrice * Quantity FROM OrderDetails ORDER BY OrderID, ProductID
```

## 14. How many customers?

How many customers do we have in the Customers table?

Show one value only, and don’t rely on getting the recordcount at the end of a resultset.

```sql
SELECT COUNT(CustomerID) FROM customers
```

## 15. When was the first order?

Show the date of the first order ever made in the Orders table.

```sql
SELECT MIN(OrderDate) FROM Orders
```

## 16. Countries where there are customers

Show a list of countries where the Northwind company has customers.

```sql
SELECT Country FROM Customers GROUP BY Country
```

## 17. Contact titles for customers

Show a list of all the different values in the Customers table for ContactTitles. Also include a count for each ContactTitle.

This is similar in concept to the previous question “Countries where there are customers”, except we now want a count for each ContactTitle.

```sql
SELECT ContactTitle, TotalContactTitle = COUNT(ContactTitle) FROM Customers GROUP BY ContactTitle ORDER BY TotalContactTitle DESC
```

## 18. Products with associated supplier names

We’d like to show, for each product, the associated Supplier. Show the ProductID, ProductName, and the CompanyName of the Supplier. Sort by ProductID. This question will introduce what may be a new concept, the Join clause in SQL. The Join clause is used to join two or more relational database tables together in a logical way.

```sql
SELECT ProductID, ProductName, CompanyName FROM Products JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID ORDER BY ProductID
```

## 19. Orders and the Shipper that was used

We’d like to show a list of the Orders that were made, including the Shipper that was used. Show the OrderID, OrderDate (date only), and CompanyName of the Shipper, and sort by OrderID.

In order to not show all the orders (there’s more than 800), show only those rows with an OrderID of less than 10300.

```sql
SELECT OrderID, OrderDate = CONVERT(date, OrderDate), CompanyName FROM Orders JOIN Shippers ON Orders.shipVia = shippers.ShipperID WHERE OrderID < 10300
```

# Introductory Problems

## 20. Categories, and the total products in each category
For this problem, we’d like to see the total number of products in each category. Sort the results by the total number of products, in descending order.
```sql
SELECT CategoryName, COUNT(ProductID) as 'No of Products' FROM Categories JOIN Products ON Categories.CategoryID = Products.CategoryID GROUP BY CategoryName ORDER BY 'No of Products' DESC
```

## 21. Total customers per country/city
In the Customers table, show the total number of customers per Country and City.
```sql
SELECT Country, City, COUNT(City) as TotalCustomer FROM Customers GROUP BY Country, City ORDER BY TotalCustomer DESC
```

## 22. Products that need reordering
What products do we have in our inventory that should be reordered? For now, just use the fields UnitsInStock and ReorderLevel, where UnitsInStock is less than the ReorderLevel, ignoring the fields UnitsOnOrder and Discontinued.
Order the results by ProductID.
```sql
SELECT ProductID, ProductName, UnitsInStock, ReorderLevel FROM Products Where UnitsInStock < ReorderLevel ORDER BY ProductID
```