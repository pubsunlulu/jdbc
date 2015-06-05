# Statement对象

一旦我们获得了数据库的连接，我们就可以和数据库进行交互。 JDBC 的  Statement ,  CallableStatement 和 PreparedStatement 接口定义的方法和属性，可以让你发送 SQL 命令或 PL / SQL 命令到数据库，并从你的数据库接收数据。

在数据库中，它们还定义了帮助 Java 和 SQL 数据类型之间转换数据差异的方法。

下表提供了每个接口的用途概要，根据实际目的决定使用哪个接口。

<table class="table table-bordered">
<tr>
<th style="width:28%">接口</th>
<th style="width:72%">推荐使用</th>
</tr>
<tr>
<td>Statement</td>
<td>可以正常访问数据库，适用于运行静态 SQL 语句。 Statement 接口不接受参数。</td>
</tr>
<tr>
<td>PreparedStatement</td>
<td>计划多次使用 SQL 语句， PreparedStatement 接口运行时接受输入的参数。</td>
</tr>
<tr>
<td>CallableStatement</td>
<td>适用于当你要访问数据库存储过程的时候， CallableStatement 接口运行时也接受输入的参数。</td>
</tr>
</table>

## Statement 对象

### 创建 Statement 对象

在你准备使用 Statement 对象执行 SQL 语句之前，你需要使用  Connection 对象的 createStatement() 方法创建一个，如下面的示例所示-

```

Statement stmt = null;
try {
   stmt = conn.createStatement( );
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}


``` 

当你创建了一个 Statement 对象之后，你可以用它的三个执行方法的任一方法来执行 SQL 语句。

- **boolean execute(String SQL) :** 如果 ResultSet 对象可以被检索，则返回的布尔值为 true ，否则返回 false 。当你需要使用真正的动态 SQL 时，可以使用这个方法来执行 SQL DDL 语句。
- **int executeUpdate(String SQL) :** 返回执行 SQL 语句影响的行的数目。使用该方法来执行 SQL 语句，是希望得到一些受影响的行的数目，例如， INSERT ， UPDATE 或 DELETE 语句。
- **ResultSet executeQuery(String SQL) :** 返回一个  ResultSet 对象。当你希望得到一个结果集时使用该方法，就像你使用一个 SELECT 语句。

### 关闭 Statement 对象

正如你关闭一个 Connection 对象来节约数据库资源，出于同样的原因你也应该关闭 Statement 对象。

简单的调用 close() 方法就可以完成这项工作。如果你关闭了  Connection 对象，那么它也会关闭 Statement 对象。然而，你应该始终明确关闭 Statement 对象，以确保真正的清除。

```

Statement stmt = null;
try {
   stmt = conn.createStatement( );
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   stmt.close();
}


```

为了更好地理解，我们建议你研究学习 **Statement 示例教程**。

## PreparedStatement 对象

*PreparedStatement* 接口扩展了 Statement 接口，它让你用一个常用的 Statement 对象增加几个高级功能。

这个 statement 对象可以提供灵活多变的动态参数。

### 创建 PreparedStatement 对象

```

PreparedStatement pstmt = null;
try {
   String SQL = "Update Employees SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}


```

JDBC 中所有的参数都被用 **？** 符号表示，这是已知的参数标记。在执行 SQL 语句之前，你必须赋予每一个参数确切的数值。

**setXXX()** 方法将值绑定到参数，其中 **XXX** 表示你希望绑定到输入参数的 Java 数据类型。如果你忘了赋予值，你将收到一个 SQ 异常。

每个参数标记映射它的序号位置。第一标记表示位置 1 ，下一个位置为 2  等等。这种方法不同于 Java 数组索引，它是从 0 开始的。

所有的 **Statement对象** 的方法都与数据库交互， (a) execute() , (b) executeQuery() , 及 (c) executeUpdate() 也能被  PreparedStatement 对象引用。然而，这些方法被 SQL 语句修改后是可以输入参数的。

### 关闭 PreparedStatement 对象

正如你关闭一个 Statement 对象，出于同样的原因，你也应该关闭  PreparedStatement 对象。

简单的调用 close() 方法可以完成这项工作。如果你关闭了 Connection  对象，那么它也会关闭 PreparedStatement 对象。然而，你应该始终明确关闭 PreparedStatement 对象，以确保真正的清除。

```

PreparedStatement pstmt = null;
try {
   String SQL = "Update Employees SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   pstmt.close();
}


```

为了更好地理解，让我们研究学习 **Prepare - 示例代码**

## CallableStatement 对象

正如一个 Connection 对象可以创建 Statement 对象和  PreparedStatement 对象，它也可以创建被用来执行调用数据库存储过程的 CallableStatement 对象。

### 创建 CallableStatement 对象

假如你需要执行以下的 Oracle 存储过程-

```

CREATE OR REPLACE PROCEDURE getEmpName 
   (EMP_ID IN NUMBER, EMP_FIRST OUT VARCHAR) AS
BEGIN
   SELECT first INTO EMP_FIRST
   FROM Employees
   WHERE ID = EMP_ID;
END;


```

**注意：**上面的存储过程已经写入到 Oracle 数据库中，但我们正在使用 MySQL 数据库，那么我们可以在 MySQL 的 EMP 数据库中创建相同的存储过程。

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

三种类型的参数有： IN ， OUT 和 INOUT 。  PreparedStatement 对象只使用 IN 参数。  CallableStatement 对象可以使用所有的三个参数。

这里是每个参数的定义-

<table class="table table-bordered">
<tr>
<th style="width:28%">参数</th>
<th style="width:72%">描述</th>
</tr>
<tr>
<td>IN</td>
<td>在 SQL 语句创建的时候该参数是未知的。你可以用 setXXX() 方法将值绑定到IN参数中。</td>
</tr>
<tr>
<td>OUT</td>
<td>该参数由 SQL 语句的返回值提供。你可以用 getXXX() 方法获取  OUT 参数的值。</td>
</tr>
<tr>
<td>INOUT</td>
<td>该参数同时提供输入输出的值。你可以用 setXXX() 方法将值绑定参数，并且用 getXXX() 方法获取值。.</td>
</tr>
</table>

下面的代码片段展示了基于存储过程如何使用  **Connection.prepareCall()** 方法来实例化  **CallableStatement** 对象。

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

SQL 的 String 变量使用参数占位符表示存储过程。

使用 CallableStatement 对象就像使用 PreparedStatement 对象。你必须在执行该语句之前将值绑定到所有的参数，否则你将收到一个 SQL 异常。

如果你有 IN 参数，只要使用适用于 PreparedStatement 对象相同的规则和技巧;使用 setXXX() 方法绑定对应的 Java 数据类型。

当你使用 OUT 和 INOUT 参数时，你就必须使用额外的   CallableStatement 方法 - registerOutParameter() 。  registerOutParameter() 方法绑定 JDBC 数据类型，该数据是存储过程返回的值。

一旦你调用存储过程，你可以用适当的 getXXX() 方法来获取 OUT 参数的值。这个方法将检索到的 SQL 类型映射成 Java 数据类型。

### 关闭 CallableStatement 对象

正如你关闭其它的 Statement 对象，出于同样的原因，你也应该关闭   PreparedStatement 对象。

简单的调用 close() 方法可以完成这项工作。如果你关闭了 Connection 对象，那么它也会关闭 CallableStatement 对象。然而，你应该始终明确关闭 CallableStatement 对象，以确保真正的清除。

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

为了更好地理解，我建议你研究学习 **Callable - 示例代码**