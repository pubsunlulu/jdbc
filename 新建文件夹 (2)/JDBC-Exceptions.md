异常处理可以允许你处理一个异常情况，例如可控方式的程序定义错误。

当异常情况发生时，将抛出一个异常。抛出这个词意味着当前执行的程序停止，控制器被重定向到最近的适用的 catch 子句。如果没有适用的 catch  子句存在，那么程序执行被终止。

JDBC 的异常处理是非常类似于 Java 的异常处理，但对于 JDBC ，最常见的异常是 **java.sql.SQLException**。

# SQLException 方法 #

SQLException 异常在驱动程序和数据库中都可能出现。当出现这个异常时， SQLException 类型的对象将被传递到 catch 子句。

传递的 SQLException 对象具有以下的方法，以下的方法可用于检索该异常的额外信息-

<table class="table table-bordered">
<tr>
<th style="width:42%">方法</th>
<th style="width:58%">描述</th>
</tr>
<tr>
<td>getErrorCode( )</td>
<td>获取与异常关联的错误号。</td>
</tr>
<tr>
<td>getMessage( )</td>
<td>获取 JDBC 驱动程序的错误信息，该错误是由驱动程序处理的，或者在数据库错误中获取 Oracl 错误号和错误信息。</td>
</tr>
<tr>
<td>getSQLState( )</td>
<td>获取 XOPEN SQLstate 字符串。对于 JDBC 驱动程序错误，使用该方法不能返回有用的信息。对于数据库错误，返回第五位的 XOPEN SQLstate 代码。该方法可以返回 null 。</td>
</tr>
<tr>
<td>getNextException( )</td>
<td>获取异常链的下一个 Exception 对象。</td>
</tr>
<tr>
<td>printStackTrace( )</td>
<td>打印当前异常或者抛出，其回溯到标准的流错误。</td>
</tr>
<tr>
<td>printStackTrace(PrintStream s)</td>
<td>打印该抛出，其回溯到你指定的打印流。</td>
</tr>
<tr>
<td>printStackTrace(PrintWriter w)</td>
<td>打印该抛出，其回溯到你指定的打印写入。</td>
</tr>
</table>

通过利用可从 Exception 对象提供的信息，你可以捕获异常并继续运行程序。这是一个 try 块的一般格式-

```
try {
   // Your risky code goes between these curly braces!!!
}
catch(Exception ex) {
   // Your exception handling code goes between these 
   // curly braces, similar to the exception clause 
   // in a PL/SQL block.
}
finally {
   // Your must-always-be-executed code goes between these 
   // curly braces. Like closing database connection.
}
```

## 示例 ##

研究学习下面的示例代码来了解 **try .... catch ... finally** 块的使用。

```
//STEP 1. Import required packages
import java.sql.*;

public class JDBCExample {
   // JDBC driver name and database URL
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
   static final String DB_URL = "jdbc:mysql://localhost/EMP";

   //  Database credentials
   static final String USER = "username";
   static final String PASS = "password";
   
   public static void main(String[] args) {
   Connection conn = null;
   try{
      //STEP 2: Register JDBC driver
      Class.forName("com.mysql.jdbc.Driver");

      //STEP 3: Open a connection
      System.out.println("Connecting to database...");
      conn = DriverManager.getConnection(DB_URL,USER,PASS);

      //STEP 4: Execute a query
      System.out.println("Creating statement...");
      Statement stmt = conn.createStatement();
      String sql;
      sql = "SELECT id, first, last, age FROM Employees";
      ResultSet rs = stmt.executeQuery(sql);

      //STEP 5: Extract data from result set
      while(rs.next()){
         //Retrieve by column name
         int id  = rs.getInt("id");
         int age = rs.getInt("age");
         String first = rs.getString("first");
         String last = rs.getString("last");

         //Display values
         System.out.print("ID: " + id);
         System.out.print(", Age: " + age);
         System.out.print(", First: " + first);
         System.out.println(", Last: " + last);
      }
      //STEP 6: Clean-up environment
      rs.close();
      stmt.close();
      conn.close();
   }catch(SQLException se){
      //Handle errors for JDBC
      se.printStackTrace();
   }catch(Exception e){
      //Handle errors for Class.forName
      e.printStackTrace();
   }finally{
      //finally block used to close resources
      try{
         if(conn!=null)
            conn.close();
      }catch(SQLException se){
         se.printStackTrace();
      }//end finally try
   }//end try
   System.out.println("Goodbye!");
}//end main
}//end JDBCExample
```

现在，让我们用下面的命令编译上面的代码-

```
C:\>javac JDBCExample.java
C:\>
```

当你运行 **JDBCExample** 时，如果没有问题它将展示下面的结果，否则相应的错误将被捕获并会显示错误消息-

```
C:\>java JDBCExample
Connecting to database...
Creating statement...
ID: 100, Age: 18, First: Zara, Last: Ali
ID: 101, Age: 25, First: Mahnaz, Last: Fatma
ID: 102, Age: 30, First: Zaid, Last: Khan
ID: 103, Age: 28, First: Sumit, Last: Mittal
C:\>
```

通过传递错误的数据库名称或错误的用户名或错误的密码来测试上面的示例，并检查结果。
