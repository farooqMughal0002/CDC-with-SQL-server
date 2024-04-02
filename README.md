# CDC-with-SQL-server
Creating CDC in SQL server
-- Create a database
CREATE DATABASE New_Source_DB
USE New_Source_DB;
GO
EXEC sp_changedbowner 'ABC-PC'
GO


-- Create a Users table 
CREATE TABLE Users 
(    
   ID int NOT NULL PRIMARY KEY,    
   FirstName varchar(30),    
   LastName varchar(30),    
   Email varchar(50) 
)



-- Enable Database for Change data capture  
USE New_Source_DB  
GO  
EXEC sys.sp_cdc_enable_db  
GO




-- ======================  
-- Enable a Table for CDC 
-- ======================  
USE New_Source_DB  
GO  
  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'Users',  
@role_name     = N'ABC-PC',  
@supports_net_changes = 1  
GO





-- Check that CDC is enabled on the database
SELECT name, is_cdc_enabled
FROM sys.databases WHERE database_id = DB_ID();





INSERT INTO Users Values (1, 'Jorge', 'Ramos', 'ramos@yahoo.com')
INSERT INTO Users Values (2, 'Andrew', 'Strudwick', 'andrew@yahoo.com')
INSERT INTO USERS Values (3, 'Thomas', 'Tuchel', 'tuchel@gmail.com')





-- Query the results of the changes captured
SELECT * FROM cdc.dbo_Users_CT
GO



DELETE FROM Users WHERE ID = 1
UPDATE Users SET LastName = 'Snow' WHERE ID = 2
DELETE FROM Users WHERE ID = 3



SELECT * FROM [cdc].[dbo_Users_CT] 
GO
