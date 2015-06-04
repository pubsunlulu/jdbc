在前面的 **JDBC - Statement 对象**这章里，我们已经学习了如何在   JDBC 中使用**存储过程**。本章与该章有些类似，但本章将告诉你有关  JDBC 转义语法的额外信息。

正如一个 Connection 对象创建了 Statement 和 PreparedStatement  对象，它也创造了在数据库中被执行调用的 CallableStatement 对象。

# 创建 CallableStatement 对象 #

假设，你需要执行下面的 Oracle 存储过程-

```
CREATE OR REPLACE PROCEDURE getEmpName 
   (EMP_ID IN NUMBER, EMP_FIRST OUT VARCHAR) AS
BEGIN
   SELECT first INTO EMP_FIRST
   FROM Employees
   WHERE ID = EMP_ID;
END;
```

**注意：**上面的存储过程是在 Oracle 使用的，但我们使用的是 MySQL  数据库，所以我们在 MySQL 的环境下需要重新写出相同功能的代码，下面的代码是在 EMP 数据库中创建相同功能的代码-

```
DELIMITER $$

DROP PROCEDURE IF EXISTS `EMP`.`getEmpName` $$
CREATE PROCEDURE `EMP`.`getEmpName` 
   (IN EMP_ID INT, OUT EMP_FIRST VARCHAR(255))
BEGIN
   SELECT first INTO EMP_FIRST
   FROM Employees
   WHERE ID = EMP_ID;
END $$

DELIMITER ;
```

当前有三种类型的参数： IN ， OUT 和 INOUT 。 PreparedStatement  对象只能使用 IN 参数。  CallableStatement 对象可以使用所有的三种类型。

下面是三种类型参数的定义-

<table class="table table-bordered">

<tr>

<th style="width:28%">参数</th>

<th style="width:72%">描述</th>

</tr>

<tr>

<td>IN</td>

<td>当 SQL 语句创建的时候，该参数的值是未知的。你可以用 setXXX()  方法将值绑定到 IN 参数里。</td>

</tr>

<tr>

<td>OUT</td>

<td>该参数的值是由 SQL 语句的返回值。你可以用 getXXX() 方法从 OUT 参数中检索值。</td>

</tr>

<tr>

<td>INOUT</td>

<td>该参数同时提供输入和输出值。你可以用 setXXX() 方法将值绑定到  IN 参数里，并且也可以用 getXXX() 方法从 OUT 参数中检索值。</td>

</tr>

</table>

下面的代码片段展示了如何使用 **Connection.prepareCall（）** 方法实现一个基于上述存储过程的 **CallableStatement** 对象-

```
CallableStatement cstmt = null;
try {
   String SQL = "{call getEmpName (?, ?)}";
   cstmt = conn.prepareCall (SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}
```

字符串变量 SQL 使用参数占位符来表示存储过程。

使用 CallableStatement 对象就像使用 PreparedStatement 对象。在执行该语句前，你必须将值绑定到所有的参数，否则你将收到一个 SQL 异常。

如果你有 IN 参数，只要按照适用于 PreparedStatement 对象相同的规则和技巧;用 setXXX（） 方法来绑定对应的 Java 数据类型。

当你使用 OUT 和 INOUT 参数就必须采用额外的 CallableStatement 方法： registerOutParameter（） 。 registerOutParameter（） 方法将 JDBC 数据类型绑定到存储过程返回的数据类型。

一旦你调用了存储过程，你可以用适当的 getXXX（） 方法从 OUT 参数参数中检索数值。这种方法将检索出来的 SQL 类型的值映射到 Java 数据类型。

# 关闭 CallableStatement 对象 #

正如你关闭其它的 Statement 对象，出于同样的原因，你也应该关闭  CallableStatement 对象。

close（） 方法简单的调用就可以完成这项工作。如果你先关闭了  Connection 对象，那么它也会关闭 CallableStatement 对象。然而，你应该始终明确关闭 CallableStatement 对象，以确保该对象被彻底关闭。

```
CallableStatement cstmt = null;
try {
   String SQL = "{call getEmpName (?, ?)}";
   cstmt = conn.prepareCall (SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   cstmt.close();
}
```

为了更好的理解，建议研究学习**回收-示例代码**。

# JDBC 的 SQL 转义语法 #

转义语法使能够让你通过使用标准的 JDBC 方法和属性，来灵活的使用数据库的某些特定功能，而该特定功能对你来说本来是不可用的。

常用的 SQL 转义语法格式如下所示-

```
{keyword 'parameters'}
```

当你在编程的时候，你会发现以下的这些转义序列会非常有用的-

## d ,  t ,  ts 关键字 ##

它们能帮助确定日期，时间和时间戳的文字。众所周知，没有两个数据库管理系统的时间和日期的表现方式是相同的。该转义语法告诉驱动程序以目标数据库的格式来呈现日期或时间。例如-

```
{d 'yyyy-mm-dd'}
```

其中 yyyy = 年，mm = 月，DD = 日。使用这种语法  {d '2009-09-03'} 即是 2009 年 3 月 9 日。

下面是一个说明如何在表中插入日期的简单例子-

```
//Create a Statement object
stmt = conn.createStatement();
//Insert data ==> ID, First Name, Last Name, DOB
String sql="INSERT INTO STUDENTS VALUES" +
             "(100,'Zara','Ali', {d '2001-12-16'})";

stmt.executeUpdate(sql);
```

同样的，你可以使用以下两种语法之一，无论是 **t** 或 **ts** -

```
{t 'hh:mm:ss'}
``` 

其中 hh = 小时 ， mm = 分 ， ss = 秒。使用这种语法  {t '13:30:29'} 即是下午 1 点 30 分 29 秒。

```
{ts 'yyyy-mm-dd hh:mm:ss'}
```

这是用上述两种语法 'd' 和 't' 表示时间戳的组合语法。

## escape 关键字 ##

该关键字在 LIKE 子句中使用，来定义转义字符。当使用 SQL 通配符％，来匹配零个或多个字符时，该关键字就非常有用。例如-

```
String sql = "SELECT symbol FROM MathSymbols
              WHERE symbol LIKE '\%' {escape '\'}";
stmt.execute(sql);
```

如果你使用反斜杠字符（\）作为转义字符，你必须在 Java 字符串里使用两个反斜杠字符，因为反斜杠也是一个 Java 转义字符。

## fn 关键字 ##

该关键字代表在数据库管理系统中使用标量函数。例如，你可以使用 SQL 的   length 函数来计算字符串的长度-

```
{fn length('Hello World')}
```

这将返回11，也就是字符串'Hello World'的长度。

## call 关键字 ##

该关键字是用来调用存储过程的。例如，对于一个需要一个 IN 参数的存储过程，使用以下语法-

```
{call my_procedure(?)};
```

对于需要一个 IN 参数并返回一个 OUT 参数的存储过程，使用下面的语法-

```
{? = call my_procedure(?)};
```

## oj 关键字 ##

该关键字用来表示外部连接，其语法如下所示-

```
{oj outer-join}
```

其中 outer - join = 表 { LEFT |  RIGHT |  FULL }  OUTER JOIN  {表| outer - join }的搜索条件。例如-

```
String sql = "SELECT Employees 
              FROM {oj ThisTable RIGHT
              OUTER JOIN ThatTable on id = '100'}";
stmt.execute(sql);
```