# SQL语法

结构化查询语言（SQL）是一种标准化的语言，它可以让你对数据库进行操作，如创建项目，读取目录，更新目录和删除项目。

SQL 支持你可能会使用到的任何数据库，它可以让你编写独立于底层数据库的数据库代码。

本章介绍了 SQL，这是一个了解 JDBC 概念的先决条件。在经历了这一章后，你将能够创建，读取，更新和删除（通常被称为 CRUD 操作）一个数据库中的数据。

为了详细理解 SQL，你可以阅读我们的 **MySQL 教程**。

## 创建数据库

CREATE DATABASE 语句用于创建一个新的数据库。语法是-

```

SQL> CREATE DATABASE DATABASE_NAME;


```

### 示例
下面的 SQL 语句创建一个名为 EMP 的数据库-

```

SQL> CREATE DATABASE EMP;


```

## 删除数据库

使用 DROP DATABASE 语句用于删除现有的数据库。语法是-

```

SQL> DROP DATABASE DATABASE_NAME;


```

**注意：**要创建或删除数据库，你必须有数据库服务器的管理员权限。请注意，删除数据库会把存储在该数据库中的数据一并删除。

## 创建表

CREATE TABLE 语句用于创建新表。语法是 - 

```

SQL> CREATE TABLE TABLE_NAME
（
   COLUMN_NAME column_data_type，
   COLUMN_NAME column_data_type，
   COLUMN_NAME column_data_type
   ...
）;


```

### 示例

下面的 SQL 语句创建一个含有四列名为 Employees 的表-

```

SQL> CREATE TABLE Employees
(
   id INT NOT NULL,
   age INT NOT NULL,
   first VARCHAR(255),
   last VARCHAR(255),
   PRIMARY KEY ( id )
);


```

## 删除表

DROP TABLE 语句用于删除现有的表。语法是 - 

```

SQL> DROP TABLE table_name;


```

### 示例
下面的 SQL 语句删除名为 Employees 的表 - 

```

SQL> DROP TABLE Employees;


```

## INSERT 数据
INSERT 的语法如下所示，其中 column1， column2 等数据出现在相应的列中 - 

```

SQL> INSERT INTO table_name的VALUES（column1，column2，...）;


```

### 示例

下面的 SQL INSERT 语句将在前面创建的 Employees 数据库中插入新的一行数据 - 

```

SQL> INSERT INTO Employees VALUES（100，18，'Zara'，'Ali'）;


```

## SELECT 数据

SELECT 语句用于从数据库中检索数据。 SELECT 的语法-

```

SQL> SELECT column_name, column_name, ...
     FROM table_name
     WHERE conditions;


```

WHERE 子句可以使用 =, !=, <, >, <=, >=, BETWEEN 和 LIKE 这些比较操作符。

### 示例

下面的 SQL 语句从 Employees 表中选出 ID 列是100的年龄、第一列、最后一列这些信息

```

SQL> SELECT first, last, age FROM Employees WHERE id = 100;


```

下面的 SQL 语句从 Employees 表中选出第一列包含 Zara 字符的年龄、第一列、最后一列这些信息

```

SQL> SELECT first, last, age FROM Employees WHERE first LIKE '%Zara%';


```

## UPDATE 数据

UPDATE 语句用于更新数据。UPDATE 的语法-

```

SQL> UPDATE table_name
     SET column_name = value, column_name = value, ...
     WHERE conditions;


```

WHERE 子句可以使用 =, !=, <, >, <=, >=, BETWEEN 和 LIKE 这些比较操作符。

### 示例

下面的 SQL UPDATE 语句改变了 ID 是100的员工的age列的数据-

```

SQL> UPDATE Employees SET age=20 WHERE id=100;


```

## DELETE 数据

DELETE 语句用于从表中删除数据。DELETE 的语法-

```

SQL> DELETE FROM table_name WHERE conditions;


```

WHERE 子句可以使用 =, !=, <, >, <=, >=, BETWEEN 和 LIKE 这些比较操作符。

### 示例

下面的 SQL DELETE 语句将 Employees 表中 ID 是100的记录删除-

```

SQL> DELETE FROM Employees WHERE id=100;


```
