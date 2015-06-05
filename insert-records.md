# 插入记录实例


本章介绍了如何使用 JDBC 应用程序在表中插入记录的示例。执行下面的示例之前，请确保你已做好以下工作-

- 在运行下面的例子之前，你需要用你实际的用户名和密码去代替  *username* 和 *password* 。
- 你的 MySQL 或者其他数据库已经启动了并在运行中。

## 所需的步骤

用 JDBC 应用程序去创建一个新的数据库需要执行以下步骤-

- **导入包**：要求你包括含有需要进行数据库编程的 JDBC 类的包。大多数情况下，使用 *import java.sql.**  就足够了。
- **注册 JDBC 驱动程序**：要求你初始化驱动程序，这样你可以与数据库打开通信通道。
- **打开连接**：需要使用 *DriverManager.getConnection()* 方法创建一个 Connection 对象，它代表与数据库服务器的物理连接。
- **执行查询**：需要使用声明的类型对象建立并提交一个 SQL 语句，这样可以在表中插入记录。
- **清理环境**：依靠 JVM 垃圾收集器可以明确地回收所有的数据库资源。

## 示例代码

将下面的示例拷贝并粘帖到 JDBCExample.java 中，编译并运行它，如下所示-

```

//STEP 1. Import required packages
import java.sql.*;

public class JDBCExample {
   // JDBC driver name and database URL
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
   static final String DB_URL = "jdbc:mysql://localhost/STUDENTS";

   //  Database credentials
   static final String USER = "username";
   static final String PASS = "password";
   
   public static void main(String[] args) {
   Connection conn = null;
   Statement stmt = null;
   try{
      //STEP 2: Register JDBC driver
      Class.forName("com.mysql.jdbc.Driver");

      //STEP 3: Open a connection
      System.out.println("Connecting to a selected database...");
      conn = DriverManager.getConnection(DB_URL, USER, PASS);
      System.out.println("Connected database successfully...");
      
      //STEP 4: Execute a query
      System.out.println("Inserting records into the table...");
      stmt = conn.createStatement();
      
      String sql = "INSERT INTO Registration " +
                   "VALUES (100, 'Zara', 'Ali', 18)";
      stmt.executeUpdate(sql);
      sql = "INSERT INTO Registration " +
                   "VALUES (101, 'Mahnaz', 'Fatma', 25)";
      stmt.executeUpdate(sql);
      sql = "INSERT INTO Registration " +
                   "VALUES (102, 'Zaid', 'Khan', 30)";
      stmt.executeUpdate(sql);
      sql = "INSERT INTO Registration " +
                   "VALUES(103, 'Sumit', 'Mittal', 28)";
      stmt.executeUpdate(sql);
      System.out.println("Inserted records into the table...");

   }catch(SQLException se){
      //Handle errors for JDBC
      se.printStackTrace();
   }catch(Exception e){
      //Handle errors for Class.forName
      e.printStackTrace();
   }finally{
      //finally block used to close resources
      try{
         if(stmt!=null)
            conn.close();
      }catch(SQLException se){
      }// do nothing
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

当你运行 **JDBCExample** 时，它将展示下面的结果-

```

C:\>java JDBCExample
Connecting to a selected database...
Connected database successfully...
Inserting records into the table...
Inserted records into the table...
Goodbye!
C:\>


```