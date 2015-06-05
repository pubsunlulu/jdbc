# 选择数据库实例

本章介绍了如何使用 JDBC 应用程序选择一个数据库的示例。执行下面的示例之前，请确保你已做好以下工作-

- 在运行下面的例子之前，你需要用你实际的用户名和密码去代替  *username* 和 *password* 。
- 你的 MySQL 或者其他数据库已经启动了并在运行中。

##  所需的步骤

用 JDBC 应用程序去创建一个新的数据库需要执行以下步骤-

- **导入包**：要求你包括含有需要进行数据库编程的 JDBC 类的包。大多数情况下，使用 *import java.sql.**  就足够了。
- **注册 JDBC 驱动程序**：要求你初始化驱动程序，这样你可以与数据库打开通信通道。
- **打开连接**：需要使用 *DriverManager.getConnection()* 方法创建一个连接对象，它代表与所选择数据库的物理连接。
用数据库 URL 来选择数据库。下面的例子会连接到 **STUDENTS** 数据库。
- **清理环境**：依靠 JVM 垃圾收集器可以明确地回收所有的数据库资源。

##  示例代码

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
   try{
      //STEP 2: Register JDBC driver
      Class.forName("com.mysql.jdbc.Driver");

      //STEP 3: Open a connection
      System.out.println("Connecting to a selected database...");
      conn = DriverManager.getConnection(DB_URL, USER, PASS);
      System.out.println("Connected database successfully...");
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

当你运行 **JDBCExample** 时，它将展示下面的结果-

```

C:\>java JDBCExample
Connecting to a selected database...
Connected database successfully...
Goodbye!
C:\>


```