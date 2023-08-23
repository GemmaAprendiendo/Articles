# Some mix statements that could be useful

```
select * from information_schemas where column_name like ...

select from sys.Tables where name like...

SELECT c.name AS ColName, t.name AS TableName 

FROM sys.columns c 

    JOIN sys.tables t ON c.object_id = t.object_id 

WHERE c.name LIKE '%xact_no%' 

```

## Finding duplicates

```
select cust_no, cust_ser, b_type, bill_per,  count(1) from some_table  

where whatever = 'N'  

group by cust_no, cust_ser, b_type, bill_per 

having count(1) > 1 
```

## COUNT DISTINCT DATA
```
SELECT count(*)  

    INTO counter 

    FROM  

    (SELECT UNIQUE entity.e_name, history.h_date, history.post_date, 

           history.cust_no, history.cust_ser           

        FROM account, history, entity 

        WHERE account.cust_no  = history.cust_no 

        AND   account.cust_ser = history.cust_ser 

        AND   history.xact_no  = lXact_no 

        AND   entity.entity_id = account.c_entity_id) data 

```

## SQLs to get as values as string

```
SELECT '; ' + US.je_number  

    FROM defined US 

    FOR XML PATH('') 

  

declare @results varchar(1000) 

  
select @results = coalesce(@results + ',', '') +  convert(varchar(12),je_number) 

from defined 

order by je_number 


select @results as results 
```

## Cast a null column to a type 
```
CAST(null AS DECIMAL(38,2)) AS true_payment_plan_unpaid_amount 
```

## Searching for data in case sensitive DB

```
SELECT TOP 1000 * FROM menu_options  

where title like '%bill%' COLLATE SQL_Latin1_General_CP1_CI_AS  
```

## Transactions 
```
SQL 

            SELECT COUNT(*)  

            INTO $transactionsDebug 

            FROM sys.sysprocesses WHERE open_tran = 1 

        END SQL 

  

  

@@trancount 

  

  select   

    object_name(p.object_id) as TableName,  

    resource_type, resource_description 

from 

    sys.dm_tran_locks l 

    join sys.partitions p on l.resource_associated_entity_id = p.hobt_id  
```

