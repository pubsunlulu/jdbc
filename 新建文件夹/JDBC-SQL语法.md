结构化查询语言（SQL）是一种标准化的语言，它可以让你对数据库进行操作，如创建项目，读取目录，更新目录和删除项目。

SQL支持你可能会使用到的任何数据库，它可以让你编写独立于底层数据库的数据库代码。

本章介绍了SQL，这是一个了解JDBC概念的先决条件。在经历了这一章后，你将能够创建，读取，更新和删除（通常被称为CRUD操作）一个数据库中的数据。

为了详细理解SQL，你可以阅读我们的**MySQL教程**。

# 创建数据库 #

CREATE DATABASE语句用于创建一个新的数据库。语法是：

```
SQL> CREATE DATABASE DATABASE_NAME;
```

# 示例 #
下面的SQL语句创建一个名为EMP的数据库：

```
SQL> CREATE DATABASE EMP;
```

# 删除数据库 #

使用DROP DATABASE语句用于删除现有的数据库。语法是：

```
SQL> DROP DATABASE DATABASE_NAME;
```

**注意：**要创建或删除数据库，你必须有数据库服务器的管理员权限。请注意，删除数据库会把存储在该数据库中的数据一并删除。

# 创建表 #

CREATE TABLE语句用于创建新表。语法是 - 

```
SQL> CREATE TABLE TABLE_NAME
（
   COLUMN_NAME column_data_type，
   COLUMN_NAME column_data_type，
   COLUMN_NAME column_data_type
   ...
）;
```

# 示例 #

下面的SQL语句创建一个含有四列名为Employees的表-

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

# 删除表 #

DROP TABLE语句用于删除现有的表。语法是 - 

```
SQL> DROP TABLE table_name;
```

# 示例 #
下面的SQL语句删除名为Employees的表 - 

```
SQL> DROP TABLE Employees;
```

# INSERT 数据 #
INSERT的语法如下所示，其中column1，column2等数据出现在相应的列中 - 

```
SQL> INSERT INTO table_name的VALUES（column1，column2，...）;
```

# 示例 #

下面的SQL INSERT语句将在前面创建的Employees数据库中插入新的一行数据 - 

```
SQL> INSERT INTO Employees VALUES（100，18，'Zara'，'Ali'）;
```

# SELECT 数据 #

SELECT语句用于从数据库中检索数据。SELECT的语法-

```
SQL> SELECT column_name, column_name, ...
     FROM table_name
     WHERE conditions;
```

WHERE子句可以使用 =, !=, <, >, <=, >=, BETWEEN 和 LIKE这些比较操作符。

# 示例 #

下面的SQL语句从Employees表中选出ID列是100的年龄、第一列、最后一列这些信息

```
SQL> SELECT first, last, age FROM Employees WHERE id = 100;
```

下面的SQL语句从Employees表中选出第一列包含Zara字符的年龄、第一列、最后一列这些信息

```
SQL> SELECT first, last, age FROM Employees WHERE first LIKE '%Zara%';
```

# UPDATE 数据 #

UPDATE语句用于更新数据。UPDATE的语法-

```
SQL> UPDATE table_name
     SET column_name = value, column_name = value, ...
     WHERE conditions;
```

WHERE子句可以使用 =, !=, <, >, <=, >=, BETWEEN 和 LIKE这些比较操作符。

# 示例 #

下面的SQL UPDATE语句改变了ID是100的员工的age列的数据-

```
SQL> UPDATE Employees SET age=20 WHERE id=100;
```

# DELETE数据 #

DELETE语句用于从表中删除数据。DELETE的语法-

```
SQL> DELETE FROM table_name WHERE conditions;
```

WHERE子句可以使用 =, !=, <, >, <=, >=, BETWEEN 和 LIKE这些比较操作符。

# 示例 #

下面的SQL DELETE语句将Employees表中ID是100的记录删除-

```
SQL> DELETE FROM Employees WHERE id=100;
```
