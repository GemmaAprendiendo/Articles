# SQL BASICS

## Selects

```
            SELECT * FROM orders

            SELECT OrderID, OrderDate FROM orders

            SELECT DISTINCT ShipCity FROM orders

            SELECT COUNT(DISTINCT ShipCity) FROM orders;

            SELECT * FROM orders WHERE EmployeeId > 8;

            SELECT * FROM Customers WHERE Country='Germany' AND City='Berlin';

            SELECT * FROM Customers WHERE NOT Country='Germany';

            SELECT * FROM Customers ORDER BY Country DESC;

            SELECT * FROM Customers ORDER BY Country ASC, city DESC;

            SELECT SUM(Freight) FROM orders WHERE EmployeeId = 4

            SELECT CustomerID, CompanyName AS CN FROM Customers

            SELECT EmployeeId, SUM(Freight) FROM orders WHERE EmployeeId > 4
            GROUP BY EmployeeId

            SELECT EmployeeId, AVG(Freight) FROM orders WHERE EmployeeId > 4
            GROUP BY EmployeeId

            SELECT COUNT(*) FROM CUSTOMERS; --92
            SELECT COUNT(Region) FROM CUSTOMERS; --31
            SELECT COUNT(*) FROM CUSTOMERS WHERE Region IS NOT NULL; --31

            SELECT city, Country, COUNT(*) FROM Customers 
            WHERE CompanyName LIKE 'A%' OR Address LIKE 'A%'
            GROUP BY Country, City
            HAVING COUNT(*) > 1

            SELECT TOP 3 * FROM Customers;

            SELECT TOP 50 PERCENT * FROM Customers;

            SELECT MIN(PostalCode) AS smallestPC FROM Customers;

            SELECT * FROM Customers WHERE City LIKE 'L_n_on';

            SELECT * FROM Customers WHERE Country IN ('Germany', 'France', 'UK');

            SELECT * FROM Customers WHERE Country IN (SELECT Country FROM Suppliers);

            SELECT * FROM Employees WHERE EmployeeId BETWEEN 1 AND 5;
```

## Inserts

```
            INSERT INTO Customers 
            (CustomerID, CompanyName, ContactName, Address, City, PostalCode, Country)
            VALUES 
            ('CA', 'A', 'Tom B', 'Skagen 21', 'Stavanger', '4006', 'Norway');

            INSERT INTO Customers 
            VALUES 
            ('CAA', 'ACOMP', 'Tom B', 'Sir', 'Skagen 21', 'Stavanger', 'RE', 
            '4006', 'Norway', NULL, NULL);

            INSERT INTO Customers 
            (CustomerID, CompanyName, ContactName, Address, City, PostalCode, Country)
            VALUES 
			('CAAA', 'A', 'Tom B', 'Skagen 21', 'Stavanger', '4006', 'Norway'),
			('CAAAA', 'A', 'Tom B', 'Skagen 21', 'Stavanger', '4006', 'Norway');

            DECLARE @temp Table (
	            CompanyName varchar(200),
	            ContactName varchar(200)
            );

            INSERT INTO @temp
            SELECT CompanyName, ContactName FROM Customers
```

## Updates

```
            UPDATE Customers SET City = 'MADRID' WHERE city='Madrid'

            UPDATE Customers SET Region = LEFT(City, 2)  WHERE Region IS NULL
```

## Delete

```
            DELETE FROM Customers WHERE CompanyName='A';

            DELETE FROM Customers;
```

## Joins

### INNER JOINS

Records have to match in both tables.

```
        SELECT Orders.OrderID, Customers.ContactName
        FROM Orders
        INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;

        SELECT Orders.OrderID, Orders.CustomerId AS orderCI, 
        customers.CustomerId AS CustID, Orders.ShipVia AS ordershipvia, 
        Shippers.ShipperId,  Shippers.CompanyName
        FROM ( (Orders
	        INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
	        INNER JOIN Shippers ON Orders.ShipVia = Shippers.ShipperID);        
```

### LEFT OUTER JOIN or LEFT JOIN

Returns all records from the left table, and the matched records from the right table

```
        --just checking what customers don't have any orders.  
        SELECT * FROM Customers WHERE customerId NOT IN (select CustomerId from orders)

        --the rows that are coming up not having any orders will show up in the left join statement
        SELECT Customers.CustomerId, Customers.ContactName, Orders.OrderID
        FROM Customers
        LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
        ORDER BY Customers.CustomerId;
```

### RIGHT OUTER JOIN or RIGHT JOIN

Returns all records from the right table, and the matched records from the left table

```
        SELECT Orders.OrderID, Employees.LastName, Employees.FirstName
        FROM Orders
        RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
        ORDER BY Orders.OrderID;     
```

### FULL OUTER JOIN or FULL JOIN

Returns all records when there is a match in either left or right table

```
        SELECT Customers.CompanyName, Orders.OrderID
        FROM Customers
        FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
        ORDER BY Customers.CompanyName;
```

### SELF JOIN

```
        SELECT A.CompanyName AS CustomerName1, B.CompanyName AS CustomerName2, A.City
        FROM Customers A, Customers B
        WHERE A.CustomerID <> B.CustomerID
        AND A.City = B.City
        ORDER BY A.City;
```

### CROSS JOIN

It joins every record on one side with every record on the other side. Very large results.

```
        SELECT * FROM Customers --92 records
        SELECT * FROM Orders --830 records  

        SELECT Customers.CompanyName, Orders.OrderID -- 76360 records = 92 * 830 
        FROM Customers
        CROSS JOIN Orders;

```

### UNIONS

```
        SELECT City FROM Customers -- returns distinct values 
        UNION
        SELECT City FROM Suppliers
        ORDER BY City;

        SELECT City FROM Customers -- with duplicates 
        UNION ALL
        SELECT City FROM Suppliers
        ORDER BY City;
```



## CREATE

```
        CREATE TABLE [dbo].[Categories](
	    [CategoryId] [int] IDENTITY(1,1) NOT NULL,
	    [CategoryName] [nvarchar](15) NOT NULL,
	    [Description] [ntext] NULL,
	    [Picture] [image] NULL,
         CONSTRAINT [PK_Categories] PRIMARY KEY CLUSTERED 
        (
	        [CategoryId] ASC
        )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, 
        IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, 
        OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
        ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```

## ALTER

```
        ALTER TABLE Customers
            ADD Email varchar(255);

        ALTER TABLE Customers
            DROP COLUMN Email;

        ALTER TABLE Persons
            ALTER COLUMN DateOfBirth year; --change the type
```

## CONSTRAINTS

```

        CREATE TABLE Persons (
        ID int NOT NULL,        
        FirstName varchar(255) NOT NULL,    
        );

        CREATE TABLE Persons (
                ID int NOT NULL UNIQUE,
                LastName varchar(255) NOT NULL,        
            );

            -- for multiple columns 
            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                CONSTRAINT UC_Person UNIQUE (ID,LastName)
            );

            ALTER TABLE Persons
            ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);

            ALTER TABLE Persons
            DROP CONSTRAINT UC_Person;




            CREATE TABLE Persons (
                ID int NOT NULL PRIMARY KEY,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int
            );

            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
            );

            ALTER TABLE Persons
            ADD PRIMARY KEY (ID);

            ALTER TABLE Persons
            ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName);

            ALTER TABLE Persons
            DROP CONSTRAINT PK_Person;




            CREATE TABLE Orders (
                OrderID int NOT NULL PRIMARY KEY,
                OrderNumber int NOT NULL,
                PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
            );

            CREATE TABLE Orders (
                OrderID int NOT NULL,
                OrderNumber int NOT NULL,
                PersonID int,
                PRIMARY KEY (OrderID),
                CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
                REFERENCES Persons(PersonID)
            );

            ALTER TABLE Orders
                ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);

                ALTER TABLE Orders
                ADD CONSTRAINT FK_PersonOrder
                FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
            
            ALTER TABLE Orders
                DROP CONSTRAINT FK_PersonOrder;




```

By default, SQL server will check if a parent row has any dependent children. Meaning, if you have person id 1, and you have rows that reference through foreign keys that person id 1, you cannot just delete it and leave the orphans behind. However, you can decide what to do in that case with cascades.

```
        CREATE TABLE OrderDetailsTable ....

            CONSTRAINT FK_OrderDetails1 FOREIGN KEY (OrderId) 
            REFERENCES OrdersTable(OrderId)
            ON UPDATE NO ACTION
            ON DELETE CASCADE 
            --when the row is removed in OrdersTable for id 1, all id 1 FK 
            --records here will be deleted too 
```

### CHECK

```
CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int CHECK (Age>=18)
            );

            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                City varchar(255),
                CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
            );

            ALTER TABLE Persons
                ADD CHECK (Age>=18);

            ALTER TABLE Persons
                ADD CONSTRAINT CHK_PersonAge CHECK (Age>=18 AND City='Santander');

            ALTER TABLE Persons
                DROP CONSTRAINT CHK_PersonAge;
```

In general with constraints, you can add a WITH NOCHECK before the ADD CONSTRAINT so it doesn't fail for existing data.

## DEFAULT

```
CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                City varchar(255) DEFAULT 'Valencia'
            );

            ALTER TABLE Persons
                ADD CONSTRAINT df_City
                DEFAULT 'Alicante' FOR City;

            ALTER TABLE Persons
                ALTER COLUMN City DROP DEFAULT;

```

## Index

Indexes are used to retrieve data from the database more quickly. It increases the insertion/update time, so only used when needed for retrieval.

```
CREATE INDEX idx_lastname
                ON Persons (LastName);
            
            CREATE INDEX idx_pname
                ON Persons (LastName, FirstName);
```

## Auto Increment

```
CREATE TABLE Persons (
                Personid int IDENTITY(1,1) PRIMARY KEY, --start at 1, increment by 1
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int
            );
```

## Case

```
SELECT OrderID, Quantity,
            CASE
                WHEN Quantity > 30 THEN 'The quantity is greater than 30'
                WHEN Quantity = 30 THEN 'The quantity is 30'
                ELSE 'The quantity is under 30'
            END AS QuantityText
            FROM OrderDetails;

            SELECT CustomerName, City, Country
            FROM Customers
            ORDER BY
            (CASE
                WHEN City IS NULL THEN Country
                ELSE City
            END);
```
## NULL functions

```
SELECT ProductName, UnitPrice * (UnitsInStock + ISNULL(UnitsOnOrder, 0))
            FROM Products;

            SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
            FROM Products;

```