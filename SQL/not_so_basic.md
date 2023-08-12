# Not so Basic

```
WITH MyQuery AS (
	            SELECT * FROM Customers WHERE CompanyName LIKE 'G%'
            )

            SELECT * FROM MyQuery

            --Multiple ones
            WITH MyCustomers AS (
	            SELECT * FROM Customers WHERE CompanyName LIKE 'G%'
            ),
            MyOrders AS(
	            SELECT * FROM orders 
                WHERE CustomerId IN (SELECT CustomerId FROM MyCustomers) 
            )

            --SELECT * FROM MyCustomers  
            --comment this one out because we can only have 1 statement after the WITH

            SELECT * FROM MyOrders ORDER BY CustomerID
```

```
--only the suppliers that have products with a unit price over 25
            SELECT *
            FROM Suppliers
            WHERE EXISTS (SELECT ProductName FROM Products 
            WHERE Products.SupplierID = Suppliers.supplierID AND UnitPrice > 25);

            --Can also do WHERE NOT EXISTS

            --to check if a table exists, before doing something with it 
            IF EXISTS (SELECT * FROM sys.objects 
            WHERE OBJECT_NAME(object_id) = 'customers' AND SCHEMA_NAME(schema_id) = 'dbo' AND
	        OBJECTPROPERTY(object_id, 'IsUserTable') = 1)
            BEGIN
	            PRINT 'it exists'
            END 
            GO

            --check for DB existence
            IF NOT EXISTS (SELECT 'True' FROM sys.databases WHERE name = 'Northwind1111')
            BEGIN
	            PRINT 'it does not exists'
            END 
            GO
```

```
SELECT 'supplierID is ' + CAST(supplierId AS varchar)
            FROM
            Suppliers


            SELECT 'supplierID is ' + CONVERT(VARCHAR, supplierId) AS CONVERTED
            FROM
            Suppliers

```

```
--BY TARGET is the default if not indicated.

            MERGE TARGET_LIST AS TARGET USING SOURCE_LIST AS SOURCE	

	        ON (TARGET.P_ID = SOURCE.P_ID)
            	WHEN MATCHED
		            AND TARGET.P_NAME <> SOURCE.P_NAME		            	
	            THEN UPDATE
		            SET TARGET.P_NAME = SOURCE.P_NAME,		        
			
            	WHEN NOT MATCHED BY TARGET
	                THEN INSERT (P_ID, P_NAME, P_PRICE)		
		            VALUES (SOURCE.P_ID, SOURCE.P_NAME, SOURCE.P_PRICE)
			
	            WHEN NOT MATCHED BY SOURCE
	                THEN DELETE

            --you can also add OUTPUT at the end if you want to obtain back a result set of what happened.
```

```
--get this estra column with row ids
            SELECT 
                ROW_NUMBER() OVER ( ORDER BY EmployeeId) row_num,  
                LastName,
                FirstName,
                EmployeeId
            FROM
                employees;

            --get this estra column with row ids
            SELECT 
                ROW_NUMBER() OVER ( ORDER BY EmployeeId) row_num, 
                LastName,
                FirstName,
                EmployeeId
            FROM
                employees;
            WHERE row_num > 10

            SELECT 
                RANK() OVER ( ORDER BY Title) theRank, 
                LastName,
                FirstName,
	            EmployeeId
            FROM
                employees;

            
            /*theRank	LastName	FirstName	EmployeeId
            1	Callahan	Laura	8
            2	Buchanan	Steven	5
            3	Suyama	Michael	6
            3	King	Robert	7
            3	Dodsworth	Anne	9
            3	Davolio	Nancy	1
            3	Leverling	Janet	3
            3	Peacock	Margaret	4
            9	Fuller	Andrew	2*/

            /*Different from the RANK() function, 
            the DENSE_RANK() function always generates consecutive rank values.*/

            SELECT 
                DENSE_RANK() OVER ( ORDER BY Title) theRank, 
                LastName,
                FirstName,
	            EmployeeId
            FROM
                employees;
            /*theRank	LastName	FirstName	EmployeeId
            1	Callahan	Laura	8
            2	Buchanan	Steven	5
            3	Suyama	Michael	6
            3	King	Robert	7
            3	Dodsworth	Anne	9
            3	Davolio	Nancy	1
            3	Leverling	Janet	3
            3	Peacock	Margaret	4
            4	Fuller	Andrew	2*/

            --NTILE divides the reesults into categories.
            SELECT 
                NTILE(3)  OVER ( ORDER BY Title) theRank, 
                LastName,
                FirstName,
	            EmployeeId
            FROM
                employees;

            /*theRank	LastName	FirstName	EmployeeId
            1	Callahan	Laura	8
            1	Buchanan	Steven	5
            1	Suyama	Michael	6
            2	King	Robert	7
            2	Dodsworth	Anne	9
            2	Davolio	Nancy	1
            3	Leverling	Janet	3
            3	Peacock	Margaret	4
            3	Fuller	Andrew	2*/

            SELECT    
                LastName,
                FirstName,
                EmployeeId
            FROM
                employees
            ORDER BY EmployeeId --need this order by
            OFFSET 0 ROWS --number of rows to skip before retrieving.
            FETCH NEXT 2 ROW ONLY
```

```
create view [dbo].[Current Product List] AS
            SELECT Product_List.ProductID, Product_List.ProductName
            FROM Products AS Product_List
            WHERE (((Product_List.Discontinued)=0))

            SELECT * FROM [Current Product List] -- [] because it has spaces

            CREATE VIEW emp2 (first___name , last___name) AS
            SELECT 
                FirstName, lastName
            FROM
                employees   
            GO

            SELECT *  FROM emp2


            DROP VIEW emp2 
            GO


            ALTER VIEW emp2 (firstname___ , lastname___) AS
            SELECT 
                FirstName, lastName
            FROM
                employees   
            GO


            ALTER VIEW emp2 (firstname___ , lastname___) AS
            SELECT 
                FirstName, lastName
            FROM
                employees   
	        WHERE city = 'London'
	        WITH CHECK OPTION -- if using the view for inserts, only if the row satisfies the WHERE
            GO

            --to see the view code 
            EXEC sp_helptext emp2

            --use WITH ENCRYPTION and the sp_helptext will not show the code.
            ALTER VIEW theview WITH ENCRYPTION AS ... 
```

```
USE northwind;
            DECLARE @empid int;
            SET @empid = 0; --init
            SET @empid = (SELECT MAX(EmployeeId) FROM Employees ); --get it
            SELECT @empid; --display it

            DECLARE @empid int = 0;
```

## Some System Functions

```
@@DATEFIRST --> what is currently set as the first day of the week

@@ERROR --> error number of last T-SQL statement executed. O for no error

@@IDENTITY --> value of last identity value inserted, or of select into.

IDENT_CURRENT('tablename') --> last identity value but for specific table

@@OPTIONS --> info about options that have been set with SET

@@REMSERVER --> value of the server that called the stored procedure.

@@ROWCOUNT --> Number of rows affected by the last statement

SCOPE_IDENTITY() --> last idenitty inserted within the current session and scope

@@SERVERNAME --> name of local server

@@TRANCOUNT --> number of active transactions for the current connection

@@VERSION --> current version of SQL server
```

## Flow Statements

```
IF ... ELSE

BEGIN ... END

CASE ... WHEN ... THEN ... ELSE...END

WHILE ... BEGIN ... END

WAIT FOR ... DELAY

BEGIN TRY ... END TRY BEGIN CATCH ... END CATCH
```

## Stored Procedures and functions

```
CREATE PROCEDURE SelectAllCustomers
            AS
                SELECT * FROM Customers
            GO;
            EXEC SelectAllCustomers;


            CREATE PROCEDURE SelectSomeCustomers @City nvarchar(30)
            AS
                SELECT * FROM Customers WHERE City = @City
            GO;
            EXEC SelectSomeCustomers @City = 'London'; 

            CREATE PROCEDURE SelectSomeCustomers @City nvarchar(30), @Country nvarchar(30)
            AS
                SELECT * FROM Customers WHERE City = @City AND Country=@Country;
            GO;
            EXEC SelectSomeCustomers @City = 'London', @Country="UK" ; 


            DROP PROC SelectSomeCustomers;


            CREATE PROCEDURE SomeProcedure
                @SalesPerson nvarchar(50),  
                @SalesYTD money OUTPUT  
            AS                                
                SELECT @SalesYTD = SalesYTD  
                FROM SalesPerson 
                WHERE ...

                RETURN;
            GO 
```

Stored Procedures are not really intended to return data, but user functions can return scalar as well as table data.
Created similar to the procedure, but use CREATE FUNCTION instead

```
CREATE FUNCTION someFunc (@dt DATE)
            RETURNS BIT
            AS 
            BEGIN 
                DECLARE @out BIT =0;
                ... 
                RETURN @out 
            END

            CREATE FUNCTION someFunc (@dt DATE)
            RETURNS TABLE
            AS 
            BEGIN 
                ... 
                RETURN (SELECT ...)
            END
            GO
```

## Keywords to deal with transactions

```
BEGIN TRAN
COMMIT TRAN
ROLLBACK TRAN
SAVE TRAN(for partial rollbacks)
```

## Triggers

set a trigger for what happens when a table gets an insert, delete, update
```
--Could do INSTEAD OF DELETE for when a delete happens
            DROP TRIGGER IF EXISTS t_for_insert;
            GO
            CREATE TRIGGER t_for_insert ON country INSTEAD OF INSERT 
            AS BEGIN            
                DECLARE @country_name_eng CHAR(128);            
                DECLARE @country_name CHAR(128);        
                SELECT @country_name = country_name FROM INSERTED;            
                IF @country_name_eng IS NULL SET @country_name_eng = @country_name;
                INSERT INTO country (country_name_eng) VALUES (@country_name_eng);
            END;

            --could throw an error and prevent the insert , delete, update:
            THROW 51000, "no way!";

            --a trigger can also be AFTER INSERT (same as FOR INSERT).
            --in an INSTEAD OF INSERTED, 
            --the INSERTED table lives only for as long as the trigger is happening.
            --if we are doing the INSTEAD OF DELETE, 
            --what we have is a DELETED table to check.
            --for an update, we don't have an UPDATED table, we have the same row in a DELETED and an INSERTED table.
            --the trigger happens before any changes are committed.
```

## Cursors

```
DECLARE @name VARCHAR(50) -- database name 
            SET @name = 'default name'
            --or whatever you need                                     
            
            DECLARE db_cursor CURSOR FOR
                SELECT name 
                FROM someTable
            WHERE ...--whatever
            
            OPEN db_cursor   
            
            FETCH NEXT FROM db_cursor INTO @name  
            
            WHILE @@FETCH_STATUS = 0              
                BEGIN  
                    --do whatever code you need
                    -- Fetch the next record from the cursor
                    FETCH NEXT FROM db_cursor INTO @name 
                END 
            
            CLOSE db_cursor              
            DEALLOCATE db_cursor 
```

## Temp Table

Create a temp table by creating a table but naming it with #YourTableName. Use ## if you want it to be available from all sessions.

## DECODE

allows you to add procedure if-then-else logic to queries

`SELECT DECODE(1,1,'SAME');`

compares 1 to 1 and if they are the same returns SAME.  If they were not equal, it would return null.

If you also need code for the else you would `SELECT DECODE(1,2, 'SAME', 'NOT THE SAME');`

You can also have an if elseif with a list or arguments:

`SELECT DECODE (2, 1, 'same as 1', 2, 'same as 2');`

`SELECT DECODE(3,1, 'same as 1,', 2, 'same as 2', 'not same as anything');`

## Get latest date or whatever in a table.


