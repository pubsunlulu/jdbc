在你安装相应的驱动程序后，就可以用JDBC建立一个数据库连接。

编写建立一个 JDBC 连接的程序是相当简单的。下面是简单的四个步骤-

- **导入 JDBC 包**：在你的 Java 代码中，用 **import** 语句添加你所需的类。
- **注册 JDBC 驱动程序**：这一步会导致 JVM 加载所需的驱动程序到内存中执行，因此它可以实现你的 JDBC 请求。
- **数据库 URL 制定**：这是用来创建格式正确的地址指向你想要连接的数据库。
- **创建连接对象**：最后，代码调用 *DriverManager* 对象的  *getConnection()* 方法来建立实际的数据库连接。

# 导入 JDBC 包 #

**import** 语句告诉 Java 编译器在哪里可以找到你在代码中引用的类，这些引用放置在你的源代码起始位置。

使用标准的 JDBC 包，它允许你选择，插入，更新和删除 SQL 表中的数据，添加以下引用到您的源代码中-

```
import java.sql.* ;  // for standard JDBC programs
import java.math.* ; // for BigDecimal and BigInteger 
```

# 注册 JDBC 驱动程序 #

在使用驱动程序之前，你必须在你的程序里面注册它。通过加载 Oracle 驱动程序的类文件到内存中来注册驱动程序，因此它可以采用 JDBC 接口来实现。

你需要在你的程序里做一次注册即可。莫可以通过以下两种方式来注册一个驱动程序。

## 方法（I）- Class.forName() ##

注册一个驱动程序中最常用的方法是使用 Java 的 **Class.forName()** 方法来动态加载驱动程序的类文件到内存中，它会自动将其注册。这种方法更优越一些，因为它允许你对驱动程序的注册信息进行配置，便于移植。

下面是使用 Class.forName() 来注册 Oracle 驱动程序的示例：

```
try {
   Class.forName("oracle.jdbc.driver.OracleDriver");
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
}
```

你可以使用 **getInstance()** 方法来解决不兼容的 JVM ，但你必须编写如下所示的两个额外的异常-

```
try {
   Class.forName("oracle.jdbc.driver.OracleDriver").newInstance();
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
catch(IllegalAccessException ex) {
   System.out.println("Error: access problem while loading!");
   System.exit(2);
catch(InstantiationException ex) {
   System.out.println("Error: unable to instantiate driver!");
   System.exit(3);
}
```

## 方法（二） - DriverManager.registerDriver() ##

你注册一个驱动程序的第二种方法是使用静态  **staticDriverManager.registerDriver()** 方法。

如果你使用的是不兼容 JVM 的非 JDK ，比如微软提供的，你必须使用  *registerDriver()* 方法。

下面是使用 registerDriver() 来注册 Oracle 驱动程序的示例：

```
try {
   Driver myDriver = new oracle.jdbc.driver.OracleDriver();
   DriverManager.registerDriver( myDriver );
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
}
```

# 数据库 URL 制定 #

当你加载了驱动程序之后，你可以通过  **DriverManager.getConnection()** 方法建立一个连接。为方便参考，以下列出了三个加载 DriverManager.getConnection() 方法：

- getConnection(String url)
- getConnection(String url, Properties prop)
- getConnection(String url, String user, String password)

在这里，每个格式需要一个数据库 **URL** ，数据库 URL 是指向数据库的地址。

在建立一个数据连接的时候，大多数会在配置一个数据库 URL 时遇到问题。

下表列出了常用的 JDBC 驱动程序名和数据库URL。

<table class="table table-bordered">
<tr>
<th style="width:10%">RDBMS</th>
<th style="width:35%">JDBC 驱动程序名称</th>
<th>URL 格式</th>
</tr>
<tr>
<td>MySQL</td>
<td>com.mysql.jdbc.Driver</td>
<td><b>jdbc:mysql://</b>hostname/ databaseName</td>
</tr>
<tr>
<td>ORACLE</td>
<td>oracle.jdbc.driver.OracleDriver</td>
<td><b>jdbc:oracle:thin:@</b>hostname:port Number:databaseName</td>
</tr>
<tr>
<td>DB2</td>
<td>COM.ibm.db2.jdbc.net.DB2Driver</td>
<td><b>jdbc:db2:</b>hostname:port Number/databaseName</td>
</tr>
<tr>
<td>Sybase</td>
<td>com.sybase.jdbc.SybDriver</td>
<td><b>jdbc:sybase:Tds:</b>hostname: port Number/databaseName</td>
</tr>
</table>


URL 格式所有加粗的部分都是静态的，你需要将剩余部分按照你的数据库实际情况进行设置。

# 创建连接对象 #

我们已经列出了三种用 **DriverManager.getConnection()** 方法的方式来创建一个连接对象。

## 使用数据库 URL 的用户名和密码 ##

getConnection() 最常用的方式是需要你提供一个数据库 URL ，用户名和密码： 

假设你使用的是 Oracle 的简化驱动程序，你可以从 URL 获得数据库的主机名：端口：数据库名称的信息。

如果你有一台名为 amrood 的主机，它的 TCP / IP 地址 192.0.0.1，你的 Oracle 监听器被配置为监听端口 1521，数据库名称是 EMP ，然后完整的数据库 URL 是-

```
jdbc:oracle:thin:@amrood:1521:EMP
```

现在，你必须调用适当的用户名和密码以及 getConnection() 方法来获得一个 **Connection** 对象，如下所示：

```
String URL = "jdbc:oracle:thin:@amrood:1521:EMP";
String USER = "username";
String PASS = "password"
Connection conn = DriverManager.getConnection(URL, USER, PASS);
```

## 只使用数据库 URL ##

第二种 DriverManager.getConnection() 方法调用的方式只需要数据库 URL 参数-

```
DriverManager.getConnection(String url);
```

然而，在这种情况下，数据库的 URL ，包括用户名和密码，将表现为以下的格式-

```
jdbc:oracle:driver:username/password@database
```

所以上述连接对象可以如下所示创建连接-

```
String URL = "jdbc:oracle:thin:username/password@amrood:1521:EMP";
Connection conn = DriverManager.getConnection(URL);
```

## 使用数据库 URL 和 Properties 对象 ##

第三种 DriverManager.getConnection() 方法调用需要数据库 URL 和 Properties 对象-

```
DriverManager.getConnection(String url, Properties info);
```

Properties 对象保存了一组关键数值。它通过调用 getConnection() 方法，将驱动程序属性传递给驱动程序。

使用下面的代码可以建立与上述示例相同的连接-

```
import java.util.*;

String URL = "jdbc:oracle:thin:@amrood:1521:EMP";
Properties info = new Properties( );
info.put( "user", "username" );
info.put( "password", "password" );

Connection conn = DriverManager.getConnection(URL, info);
```

# 关闭JDBC连接 #

在 JDBC 程序的末尾，它必须明确关闭所有的连接到数据库的连接，以结束每个数据库会话。但是，如果忘了， Java 垃圾收集器也会关闭连接，它会完全清除过期的对象。

依托垃圾收集器，特别是在数据库编程，是非常差的编程习惯。你应该养成用 close() 方法关闭连接对象的习惯。

为了确保连接被关闭，你可以在代码中的 ' finally ' 程序块中执行。 无论异常是否发生， *finally* 程序是肯定会被执行的。

要关闭上面打开的连接，你应该调用 close() 方法，如下所示-

```
conn.close();
```

对你的数据库管理员来说，明确的关闭连接到 DBMS 的连接，是相当开心的事。

为了更好地理解，我们建议你研究学习我们的 JDBC - 示例代码教程。