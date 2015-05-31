如果你的JDBC连接是处于自动提交模式下，该模式为默认模式，那么每句SQL语句都是在其完成时提交到数据库。

对简单的应用程序来说这种模式相当好，但有三个原因你可能想关闭自动提交模式，并管理你自己的事务-

- 为了提高性能
- 为了保持业务流程的完整性
- 使用分布式事务

你可以通过事务在任意时间来控制以及更改应用到数据库。它把单个SQL语句或一组SQL语句作为一个逻辑单元，如果其中任一语句失败，则整个事务失败。

若要启用手动事务模式来代替JDBC驱动程序默认使用的自动提交模式的话，使用Connection对象的的**setAutoCommit()**方法。如果传递一个布尔值false到setAutoCommit()方法，你就关闭自动提交模式。你也可以传递一个布尔值true将其再次打开。

例如，如果有一个名为conn的Connection对象，以下的代码将关闭自动提交模式-

```
conn.setAutoCommit(false);
```

# 提交和回滚 #

当你完成了你的修改，并且要提交你的修改，可以在connection对象里调用**commit（）**方法，如下所示-

```
conn.commit( );
```

另外,用名为conn的连接回滚数据到数据库，使用如下所示的代码-

```
conn.rollback( );
```

下面的例子说明了如何使用提交和回滚对象-

```
try{
   //Assume a valid connection object conn
   conn.setAutoCommit(false);
   Statement stmt = conn.createStatement();
   
   String SQL = "INSERT INTO Employees  " +
                "VALUES (106, 20, 'Rita', 'Tez')";
   stmt.executeUpdate(SQL);  
   //Submit a malformed SQL statement that breaks
   String SQL = "INSERTED IN Employees  " +
                "VALUES (107, 22, 'Sita', 'Singh')";
   stmt.executeUpdate(SQL);
   // If there is no error.
   conn.commit();
}catch(SQLException se){
   // If there is any error.
   conn.rollback();
}
```

在这种情况下，之前的INSERT语句不会成功，一切都将被回滚到最初状态。
为了更好地理解，让我们研究学习事务提交-示例代码。

# 使用还原点 #

新的JDBC3.0还原点接口提供了额外的事务控制。大部分现代的数据库管理系统的环境都支持设定还原点，例如Oracle的PL/ SQL。

当你在事务中设置一个还原点来定义一个逻辑回滚点。如果在一个还原点之后发生错误，那么可以使用rollback方法来撤消所有的修改或在该还原点之后所做的修改。

Connection对象有两个新的方法来管理还原点-

- **setSavepoint(String savepointName):** 定义了一个新的还原点。它也返回一个Savepoint对象。
- **releaseSavepoint(Savepoint savepointName):** 删除一个还原点。请注意，它需要一个作为参数的Savepoint对象。这个对象通常是由setSavepoint()方法生成的一个还原点。

有一个**rollback (String savepointName)** 方法，该方法可以回滚到指定的还原点。 

下面的例子说明了如何使用Savepoint对象-

```
try{
   //Assume a valid connection object conn
   conn.setAutoCommit(false);
   Statement stmt = conn.createStatement();
   
   //set a Savepoint
   Savepoint savepoint1 = conn.setSavepoint("Savepoint1");
   String SQL = "INSERT INTO Employees " +
                "VALUES (106, 20, 'Rita', 'Tez')";
   stmt.executeUpdate(SQL);  
   //Submit a malformed SQL statement that breaks
   String SQL = "INSERTED IN Employees " +
                "VALUES (107, 22, 'Sita', 'Tez')";
   stmt.executeUpdate(SQL);
   // If there is no error, commit the changes.
   conn.commit();

}catch(SQLException se){
   // If there is any error.
   conn.rollback(savepoint1);
}
```

在这种情况下，之前的INSERT语句不会成功，一切都将被回滚到最初状态。

为了更好地理解，让我们研究学习还原点-示例代码。

