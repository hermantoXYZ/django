---
icon: database
---

# Cheat Sheet



### **1. Basic Commands**

#### **Connecting to MySQL**

```bash
mysql -u username -p / mysql -u root -p
```

_You'll be prompted to enter the password._

#### **Show Databases**

```sql
SHOW DATABASES;
```

#### **Create a Database**

```sql
CREATE DATABASE database_name;
```

#### **Use a Database**

```sql
USE database_name;
```

#### **Show Tables**

```sql
SHOW TABLES;
```

#### **Describe a Table**

```sql
DESCRIBE table_name;
```

_Or_

```sql
DESC table_name;
```

#### **Drop Database**

```sql
DROP DATABASE database_name;
```

#### **Drop Table**

```sql
DROP TABLE table_name;
```

***

### **2. Data Types**

#### **Numeric Types**

* `INT` – Integer
* `DECIMAL(p, s)` – Fixed-point
* `FLOAT` – Floating-point
* `DOUBLE` – Double precision

#### **String Types**

* `VARCHAR(n)` – Variable-length string
* `CHAR(n)` – Fixed-length string
* `TEXT` – Large text
* `BLOB` – Binary Large Object

#### **Date and Time Types**

* `DATE` – 'YYYY-MM-DD'
* `DATETIME` – 'YYYY-MM-DD HH:MM:SS'
* `TIMESTAMP` – Automatic timestamp
* `TIME` – 'HH:MM:SS'
* `YEAR` – 'YYYY'

***

### **3. CRUD Operations**

#### **Create (INSERT)**

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

_Example:_

```sql
INSERT INTO employees (first_name, last_name, age)
VALUES ('John', 'Doe', 30);
```

#### **Read (SELECT)**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column
LIMIT number;
```

_Examples:_

*   Select all columns:

    ```sql
    SELECT * FROM employees;
    ```
*   Select with condition:

    ```sql
    SELECT first_name, last_name FROM employees WHERE age > 25;
    ```
*   Ordering and limiting:

    ```sql
    SELECT * FROM employees ORDER BY last_name ASC LIMIT 10;
    ```

#### **Update (UPDATE)**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

_Example:_

```sql
UPDATE employees
SET age = 31
WHERE first_name = 'John' AND last_name = 'Doe';
```

#### **Delete (DELETE)**

```sql
DELETE FROM table_name
WHERE condition;
```

_Example:_

```sql
DELETE FROM employees
WHERE age < 20;
```

***

### **4. Advanced Queries**

#### **JOINs**

**INNER JOIN**

Returns records with matching values in both tables.

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

**LEFT JOIN (LEFT OUTER JOIN)**

Returns all records from the left table and matched records from the right table.

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

**RIGHT JOIN (RIGHT OUTER JOIN)**

Returns all records from the right table and matched records from the left table.

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```

**FULL OUTER JOIN**

_MySQL doesn’t support FULL OUTER JOIN directly, but it can be simulated:_

```sql
SELECT columns
FROM table1
LEFT JOIN table2 ON table1.column = table2.column
UNION
SELECT columns
FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

#### **GROUP BY and HAVING**

**GROUP BY**

Aggregates data based on one or more columns.

```sql
SELECT column, COUNT(*)
FROM table_name
GROUP BY column;
```

**HAVING**

Filters groups based on aggregate functions.

```sql
SELECT column, COUNT(*)
FROM table_name
GROUP BY column
HAVING COUNT(*) > 5;
```

#### **ORDER BY**

Sorts the result set.

```sql
SELECT columns
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC];
```

#### **LIMIT**

Restricts the number of rows returned.

```sql
SELECT columns
FROM table_name
LIMIT offset, count;
```

_Or_

```sql
SELECT columns
FROM table_name
LIMIT count OFFSET offset;
```

***

### **5. Indexes**

#### **Create an Index**

```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

#### **Create a Unique Index**

```sql
CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);
```

#### **Drop an Index**

```sql
DROP INDEX index_name
ON table_name;
```

#### **View Indexes**

```sql
SHOW INDEX FROM table_name;
```

***

### **6. Constraints**

#### **Primary Key**

```sql
CREATE TABLE table_name (
    id INT NOT NULL,
    name VARCHAR(50),
    PRIMARY KEY (id)
);
```

#### **Foreign Key**

```sql
CREATE TABLE child_table (
    id INT,
    parent_id INT,
    PRIMARY KEY (id),
    FOREIGN KEY (parent_id) REFERENCES parent_table(id)
);
```

#### **Unique Constraint**

```sql
CREATE TABLE table_name (
    id INT,
    email VARCHAR(100),
    UNIQUE (email)
);
```

#### **Not Null Constraint**

```sql
CREATE TABLE table_name (
    id INT NOT NULL,
    name VARCHAR(50) NOT NULL
);
```

#### **Default Value**

```sql
CREATE TABLE table_name (
    id INT,
    status VARCHAR(20) DEFAULT 'active'
);
```

***

### **7. Functions**

#### **Aggregate Functions**

* `COUNT(column)`
* `SUM(column)`
* `AVG(column)`
* `MIN(column)`
* `MAX(column)`

_Example:_

```sql
SELECT COUNT(*), AVG(age)
FROM employees;
```

#### **String Functions**

* `CONCAT(str1, str2, ...)` – Concatenates strings
* `SUBSTRING(str, pos, length)` – Extracts a substring
* `LENGTH(str)` – Returns the length of a string
* `UPPER(str)` – Converts to uppercase
* `LOWER(str)` – Converts to lowercase

_Example:_

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```

#### **Date Functions**

* `NOW()` – Current date and time
* `CURDATE()` – Current date
* `DATE_ADD(date, INTERVAL value unit)` – Adds interval to date
* `DATEDIFF(date1, date2)` – Difference in days

_Example:_

```sql
SELECT DATE_ADD(NOW(), INTERVAL 7 DAY) AS next_week;
```

#### **Numeric Functions**

* `ROUND(number, decimals)` – Rounds a number
* `CEIL(number)` – Rounds up
* `FLOOR(number)` – Rounds down

_Example:_

```sql
SELECT ROUND(AVG(age), 2) AS average_age
FROM employees;
```

***

### **8. Views**

#### **Create a View**

```sql
CREATE VIEW view_name AS
SELECT columns
FROM table_name
WHERE condition;
```

#### **Select from a View**

```sql
SELECT * FROM view_name;
```

#### **Drop a View**

```sql
DROP VIEW view_name;
```

***

### **9. Transactions**

#### **Start a Transaction**

```sql
START TRANSACTION;
```

_Or_

```sql
BEGIN;
```

#### **Commit a Transaction**

```sql
COMMIT;
```

#### **Rollback a Transaction**

```sql
ROLLBACK;
```

***

### **10. User Management**

#### **Create a User**

```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

#### **Grant Privileges**

```sql
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'host';
```

_Or specific privileges:_

```sql
GRANT SELECT, INSERT ON database_name.table_name TO 'username'@'host';
```

#### **Flush Privileges**

```sql
FLUSH PRIVILEGES;
```

#### **Revoke Privileges**

```sql
REVOKE SELECT ON database_name.table_name FROM 'username'@'host';
```

#### **Drop a User**

```sql
DROP USER 'username'@'host';
```

***

### **11. Backup and Restore**

#### **Backup Database**

```bash
mysqldump -u username -p database_name > backup.sql
```

#### **Restore Database**

```bash
mysql -u username -p database_name < backup.sql
```

***

### **12. Miscellaneous**

#### **Set Character Set**

```sql
SET NAMES 'utf8mb4';
```

#### **Change Password**

```sql
ALTER USER 'username'@'host' IDENTIFIED BY 'new_password';
```

#### **Check Current Database**

```sql
SELECT DATABASE();
```

#### **Check Current User**

```sql
SELECT USER();
```

***

### **13. Common Shortcuts**

*   **Select all columns:**

    ```sql
    SELECT * FROM table_name;
    ```
*   **Insert multiple rows:**

    ```sql
    INSERT INTO table_name (column1, column2)
    VALUES 
      (value1a, value2a),
      (value1b, value2b),
      ...;
    ```
*   **Update with condition:**

    ```sql
    UPDATE table_name
    SET column = value
    WHERE condition;
    ```
*   **Delete with condition:**

    ```sql
    DELETE FROM table_name
    WHERE condition;
    ```

***

### **14. Performance Tips**

* **Use Indexes:** Improve query performance by indexing columns used in WHERE, JOIN, and ORDER BY clauses.
* \*_Avoid SELECT :_ Specify only the columns you need.
*   **Use EXPLAIN:** Analyze query execution plans.

    ```sql
    EXPLAIN SELECT * FROM table_name WHERE condition;
    ```
* **Optimize Joins:** Ensure joined columns are indexed.
* **Limit Results:** Use LIMIT to reduce the amount of data processed.

***

### **15. Security Best Practices**

* **Use Strong Passwords:** Ensure all MySQL users have strong, unique passwords.
* **Least Privilege Principle:** Grant only necessary permissions to users.
* **Regular Backups:** Keep regular backups and store them securely.
* **Keep MySQL Updated:** Regularly update to the latest stable version to patch vulnerabilities.
* **Use SSL/TLS:** Encrypt connections between MySQL and clients.
