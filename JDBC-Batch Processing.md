批处理是指你将关联的SQL语句组合成一个批处理，并将他们当成一个调用提交给数据库。

当你一次发送多个SQL语句到数据库时，可以减少通信的资源消耗，从而提高了性能。

- JDBC驱动程序不一定支持该功能。你可以使用*DatabaseMetaData.supportsBatchUpdates()*方法来确定目标数据库是否支持批处理更新。如果你的JDBC驱动程序支持此功能，则该方法返回值为true。
- Statement，PreparedStatement和CallableStatement的**addBatch（）**方法用于添加单个语句到批处理。**executeBatch()**方法用于启动执行所有组合在一起的语句。
- **executeBatch()**方法返回一个整数数组，数组中的每个元素代表了各自的更新语句的更新数目。
- 正如你可以添加语句到批处理中，你也可以用**clearBatch()**方法删除它们。此方法删除所有用addBatch()方法添加的语句。但是，你不能有选择性地选择要删除的语句。

# 批处理和Statement对象 #

使用Statement对象来使用批处理所需要的典型步骤如下所示-

- 使用*createStatement()*方法创建一个Statement对象。
- 使用*setAutoCommit()*方法将自动提交设为false。
- 被创建的Statement对象可以使用*addBatch()*方法来添加你想要的所有SQL语句。
- 被创建的Statement对象可以用*executeBatch()*将所有的SQL语句执行。
- 最后，使用*commit()*方法提交所有的更改。

# 示例 #

下面的代码段提供了一个使用Statement对象批量更新的例子-

```
// Create statement object
Statement stmt = conn.createStatement();

// Set auto-commit to false
conn.setAutoCommit(false);

// Create SQL statement
String SQL = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(200,'Zia', 'Ali', 30)";
// Add above SQL statement in the batch.
stmt.addBatch(SQL);

// Create one more SQL statement
String SQL = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(201,'Raj', 'Kumar', 35)";
// Add above SQL statement in the batch.
stmt.addBatch(SQL);

// Create one more SQL statement
String SQL = "UPDATE Employees SET age = 35 " +
             "WHERE id = 100";
// Add above SQL statement in the batch.
stmt.addBatch(SQL);

// Create an int[] to hold returned values
int[] count = stmt.executeBatch();

//Explicitly commit statements to apply changes
conn.commit();
```

为了更好地理解，建议研究学习**批处理-示例代码**。

# 批处理和PrepareStatement对象 #

使用prepareStatement对象来使用批处理需要的典型步骤如下所示-

- 使用占位符创建SQL语句。
- 使用任一*prepareStatement()*方法创建prepareStatement对象。
- 使用*setAutoCommit()*方法将自动提交设为false。
- 被创建的Statement对象可以使用*addBatch()*方法来添加你想要的所有SQL语句。
- 被创建的Statement对象可以用*executeBatch()*将所有的SQL语句执行。
- 最后，使用*commit()*方法提交所有的更改。

下面的代码段提供了一个使用PrepareStatement对象批量更新的示例-

```
// Create SQL statement
String SQL = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(?, ?, ?, ?)";

// Create PrepareStatement object
PreparedStatemen pstmt = conn.prepareStatement(SQL);

//Set auto-commit to false
conn.setAutoCommit(false);

// Set the variables
pstmt.setInt( 1, 400 );
pstmt.setString( 2, "Pappu" );
pstmt.setString( 3, "Singh" );
pstmt.setInt( 4, 33 );
// Add it to the batch
pstmt.addBatch();

// Set the variables
pstmt.setInt( 1, 401 );
pstmt.setString( 2, "Pawan" );
pstmt.setString( 3, "Singh" );
pstmt.setInt( 4, 31 );
// Add it to the batch
pstmt.addBatch();

//add more batches
.
.
.
.
//Create an int[] to hold returned values
int[] count = stmt.executeBatch();

//Explicitly commit statements to apply changes
conn.commit();
```

为了更好地理解，建议研究学习**批处理-示例代码**。
