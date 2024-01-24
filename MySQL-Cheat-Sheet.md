# MySQL Cheatsheet
### Jonathan Mulleda
   
## Login to MySQL from Terminal
```
 sudo mysql -u root -p
```      
## SHOW USERS
```
SELECT User, Host FROM mysql.user;
```
## CREATE USERS
```
CREATE USER 'SampleUser'@'localhost' IDENTIFIED BY 'SamplePassword';
```
## GRANT PRIVILEGES (Show Granted Privileges and Remove)
```
GRANT ALL PRIVILEGES ON * . * TO 'SampleUser'@'localhost';

GRANT SamplePermission ON DatabaseName.TableName TO 'SampleUser'@'localhost'; 
FLUSH PRIVILEGES; 

SHOW GRANTS FOR 'SampleUser'@'localhost'; 
REVOKE type_of_permission ON DatabaseName.TableName FROM 'SampleUser'@'localhost'; 
DROP USER 'SampleUser'@'localhost'; 
```   
## SHOW, DELETE & CREATE DATABASES & SELECT DATABASES
```
SHOW DATABASES; 
DROP DATABASE DatabaseName; 
CREATE DATABASE DatabaseName; 
USE DatabaseName; 
```
## CREATE a TABLE (With Columns and their Data Types)
```
CREATE TABLE SampleTable (
    ID INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    ItemName VARCHAR(25), 
    ItemCategory VARCHAR(25),
    Amount INT,
    Date DATE,
);
```
## SHOW, DELETE & DROP Tables
```
SELECT * FROM SampleTable; 
DROP TABLE SampleTable; 
```
## Insert Single and Multiple ROWS & RECORDS 
```
INSERT INTO SampleTable (ItemCategory)
VALUES ('gadgets');

INSERT INTO SampleTable (
   ItemName, 
   ItemCategory, 
   Amount,
   Date
   ) 
   VALUES
   (
   'Samsung S24 Ultra', 
   'mobile phone', 
   2000,
   '2024-24-01'
);
```
## SELECT using WHERE Clause
```
SELECT * FROM SampleTable
WHERE ItemName = 'Samsung S24 Ultra';
```
## SELECT using WHERE Clause in a Range
```
SELECT * FROM SampleTable
WHERE Date BETWEEN '2024-01-01' AND '2024-01-31';
```
## DELETE an individual ROW
```
DELETE FROM SampleTable
WHERE ID = 1;
```
## UPDATE a ROW
```
UPDATE SampleTable 
SET Amount = 1500;
```
## Add New Column and Modify
```
ALTER TABLE SampleTable
ADD COLUMN Color VARCHAR(15) AFTER Amount;
```
## Order By and Distinct
```
SELECT * FROM SampleTable
ORDER BY Amount DESC;
SELECT DISTINCT ItemName FROM SampleTable;
```
## Concatenate Columns
```
SELECT CONCAT(ItemName, " ", Color) AS Description
FROM SampleTable;
```
## Select Distinct Rows
```
SELECT DISTINCT ItemName FROM SampleTable;
```
## Use LIKE to Search
```
SELECT * FROM SampleTable
WHERE ItemName LIKE 'S%';
```
## Select using IN
```
SELECT * FROM SampleCustomers
WHERE CustomerID IN (2, 4);
```
## Create and Remove Index
```
CREATE INDEX CustomerID ON SampleCustomers;
DROP INDEX CustomerID ON SampleCustomers;
DROP INDEX PRIMARY ON SampleCustomers;
ALTER TABLE SampleCustomers DROP INDEX CustomerID;
ALTER TABLE SampleCustomers DROP PRIMARY KEY;
```
## One to Many Relationship that shows Primary Key (PK) & Foreign Key (FK) in Two Tables
Table 1
```
CREATE TABLE Movies (
    MovieID int NOT NULL,
    MovieName varchar(255) NOT NULL,
    Director varchar(255),
    Length int,
    PRIMARY KEY (MovieID)
);
```
Table 2
```
CREATE TABLE Tickets (
    TicketID int NOT NULL,
    TicketNumber int NOT NULL,
    date datetime,
    PRIMARY KEY (TicketID),
    FOREIGN KEY (MovieID) REFERENCES Movies(TicketID)
);
```
## INNER JOIN
```
SELECT SampleTable1.Col, SampleTable2.Col
FROM SampleTable1
JOIN SampleTable2 ON SampleTable1.PrimaryKey = SampleTable1.ForeignKey;
```
```
SELECT Tickets.TicketID, Tickets.MovieID, Customer.FirstName
FROM Tickets
INNER JOIN Customer ON Customer.PersonID = Tickets.PersonID
INNER JOIN Movies ON Movies.MovieID = Tickets.TicketID; 
```
## JOIN Multiple Tables

 - Tables must have relationship (with a foreign key)
 - Each table must have a common column (that contains matching values)
 - Common column of the first table must have a primary key and another common column of the second table must have a foreign key
```
SELECT SampleTable1.Col1, SampleTable1.Col2,..., 
SampleTable2.Col1, SampleTable2.Col1,..., 
SampleTable3.Col1, SampleTable3.Col1,..., 
FROM SampleTable1
INNER JOIN SampleTable2 ON SampleTable1.Table1_ID = SampleTable2.Table1_ID 
INNER JOIN SampleTable3 ON SampleTable2.Table2_ID = SampleTable3.Table2_ID; 
```

### EXAMPLE
  
 - 3 tables: Tickets, Customers, Contacts
 - Customers table joined to Tickets table with common column CustomerID
 - Contacts table joined to Customers table with common column PersonID
```
SELECT Customers.Name, Contacts.Number, Contacts.Email, Tickets.Date, Tickets.Description 
FROM Tickets
INNER JOIN Customers ON Tickets.CustomerID = Customers.CustomerID
INNER JOIN Contacts ON Customers.PersonID = Contacts.Person_ID;
```