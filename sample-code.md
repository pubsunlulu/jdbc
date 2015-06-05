# 示例代码

本章提供了如何创建一个简单 JDBC 应用程序的示例。这个示例演示如何打开一个数据库连接，执行 SQL 查询，并显示结果。

所有在这个模版示例中提到的步骤，将在本教程的后续章节中进行详细描述。

## 创建 JDBC 应用程序

构建一个 JDBC 应用程序包括以下六个步骤-

- **导入数据包：**需要你包括含有需要进行数据库编程的JDBC类的包。大多数情况下，使用 *import java.sql.**  就足够了。
- **注册 JDBC 驱动器：** 需要你初始化一个驱动器，以便于你打开一个与数据库的通信通道。
- **打开连接：**需要使用 *DriverManager.getConnection()* 方法创建一个 Connection 对象，它代表与数据库的物理连接。
- **执行查询：**需要使用类型声明的对象建立并提交一个 SQL 语句到数据库。
- **提取结果集数据：**要求使用适当的 *ResultSet.getXXX()* 方法从结果集中检索数据。
- **清理环境：**依靠 JVM 的垃圾收集来关闭所有需要明确关闭的数据库资源。

## 示例代码


当你在未来需要创建自己的 JDBC 应用程序时，本示例可以作为一个**模板**。

在前面的章节里，基于对环境和数据库安装的示例代码已经写过。

将下面的示例拷贝并粘帖到 JDBCExample.java 中，编译并运行它，如下所示-

```

//STEP 1. Import required packages
import java.sql.*;

public class FirstExample {
   // JDBC driver name and database URL
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
   static final String DB_URL = "jdbc:mysql://localhost/EMP";

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
      System.out.println("Connecting to database...");
      conn = DriverManager.getConnection(DB_URL,USER,PASS);

      //STEP 4: Execute a query
      System.out.println("Creating statement...");
      stmt = conn.createStatement();
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
         if(stmt!=null)
            stmt.close();
      }catch(SQLException se2){
      }// nothing we can do
      try{
         if(conn!=null)
            conn.close();
      }catch(SQLException se){
         se.printStackTrace();
      }//end finally try
   }//end try
   System.out.println("Goodbye!");
}//end main
}//end FirstExample


```

现在，让我们用下面的命令编译上面的代码-

```

C:\>javac JDBCExample.java
C:\>


```

当你运行 **JDBCExample** 时，它将展示下面的结果-

```

C:\>java FirstExample
Connecting to database...
Creating statement...
ID: 100, Age: 18, First: Zara, Last: Ali
ID: 101, Age: 25, First: Mahnaz, Last: Fatma
ID: 102, Age: 30, First: Zaid, Last: Khan
ID: 103, Age: 28, First: Sumit, Last: Mittal
C:\>


```