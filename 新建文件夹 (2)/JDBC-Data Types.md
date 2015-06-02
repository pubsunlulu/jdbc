JDBC 驱动程序在将 Java 数据类型发送到数据库之前，会将其转换为相应的 JDBC 类型。对于大多数数据类型都采用了默认的映射关系。例如，一个  Java int 数据类型转换为 SQL INTEGER 。通过默认的映射关系来提供驱动程序之间的一致性。

当你调用 PreparedStatement 中的 setXXX() 方法或  CallableStatement 对象或 ResultSet.updateXXX() 方法时， Java  数据类型会转换为默认的 JDBC 数据类型，如下表概述。

<table class="table table-bordered">

<tr>

<th style="width:20%">SQL</th>

<th style="width:20%">JDBC/Java</th>

<th style="width:20%">setXXX</th>

<th style="width:20%">updateXXX</th>

</tr>

<tr>

<td>VARCHAR</td>

<td>java.lang.String</td>

<td>setString</td>

<td>updateString</td>

</tr>

<tr>

<td>CHAR</td>

<td>java.lang.String</td>

<td>setString</td>

<td>updateString</td>

</tr>

<tr>

<td>LONGVARCHAR</td>

<td>java.lang.String</td>

<td>setString</td>

<td>updateString</td>

</tr>

<tr>

<td>BIT</td>

<td>boolean</td>

<td>setBoolean</td>

<td>updateBoolean</td>

</tr>

<tr>

<td>NUMERIC</td>

<td>java.math.BigDecimal</td>

<td>setBigDecimal</td>

<td>updateBigDecimal</td>

</tr>

<tr>

<td>TINYINT</td>

<td>byte</td>

<td>setByte</td>

<td>updateByte</td>

</tr>

<tr>

<td>SMALLINT</td>

<td>short</td>

<td>setShort</td>

<td>updateShort</td>

</tr>

<tr>

<td>INTEGER</td>

<td>int</td>

<td>setInt</td>

<td>updateInt</td>

</tr>

<tr>

<td>BIGINT</td>

<td>long</td>

<td>setLong</td>

<td>updateLong</td>

</tr>

<tr>

<td>REAL</td>

<td>float</td>

<td>setFloat</td>

<td>updateFloat</td>

</tr>

<tr>

<td>FLOAT</td>

<td>float</td>

<td>setFloat</td>

<td>updateFloat</td>

</tr>

<tr>

<td>DOUBLE</td>

<td>double</td>

<td>setDouble</td>

<td>updateDouble</td>

</tr>

<tr>

<td>VARBINARY</td>

<td>byte[ ]</td>

<td>setBytes</td>

<td>updateBytes</td>

</tr>

<tr>

<td>BINARY</td>

<td>byte[ ]</td>

<td>setBytes</td>

<td>updateBytes</td>

</tr>

<tr>

<td>DATE</td>

<td>java.sql.Date</td>

<td>setDate</td>

<td>updateDate</td>

</tr>

<tr>

<td>TIME</td>

<td>java.sql.Time</td>

<td>setTime</td>

<td>updateTime</td>

</tr>

<tr>

<td>TIMESTAMP</td>

<td>java.sql.Timestamp</td>

<td>setTimestamp</td>

<td>updateTimestamp</td>

</tr>

<tr>

<td>CLOB</td>

<td>java.sql.Clob</td>

<td>setClob</td>

<td>updateClob</td>

</tr>

<tr>

<td>BLOB</td>

<td>java.sql.Blob</td>

<td>setBlob</td>

<td>updateBlob</td>

</tr>

<tr>

<td>ARRAY</td>

<td>java.sql.Array</td>

<td>setARRAY</td>

<td>updateARRAY</td>

</tr>

<tr>

<td>REF</td>

<td>java.sql.Ref</td>

<td>SetRef</td>

<td>updateRef</td>

</tr>

<tr>

<td>STRUCT</td>

<td>java.sql.Struct</td>

<td>SetStruct</td>

<td>updateStruct</td>

</tr>

</table>

JDBC 3.0 增强了对 BLOB ， CLOB ， ARRAY 和 REF 数据类型的支持。    ResultSet 对象现在有 UPDATEBLOB() ， updateCLOB() ，  updateArray() ，和 updateRef() 方法，通过这些方法你可以直接操作服务器上的相应数据。

你能用 setXXX() 方法和 updateXXX() 方法将 Java 类型转换为特定的 JDBC 数据类型。你能用 setObject() 方法和 updateObject() 方法将绝大部分的 Java 类型映射到 JDBC 数据类型。

ResultSet 对象为任一数据类型提供相应的 getXXX() 方法，该方法可以获取任一数据类型的列值。上述任一方法的使用需要列名或它的顺序位置。

<table class="table table-bordered">

<tr>

<th style="width:20%">SQL</th>

<th style="width:20%">JDBC/Java</th>

<th style="width:20%">setXXX</th>

<th style="width:20%">getXXX</th>

</tr>

<tr>

<td>VARCHAR</td>

<td>java.lang.String</td>

<td>setString</td>

<td>getString</td>

</tr>

<tr>

<td>CHAR</td>

<td>java.lang.String</td>

<td>setString</td>

<td>getString</td>

</tr>

<tr>

<td>LONGVARCHAR</td>

<td>java.lang.String</td>

<td>setString</td>

<td>getString</td>

</tr>

<tr>

<td>BIT</td>

<td>boolean</td>

<td>setBoolean</td>

<td>getBoolean</td>

</tr>

<tr>

<td>NUMERIC</td>

<td>java.math.BigDecimal</td>

<td>setBigDecimal</td>

<td>getBigDecimal</td>

</tr>

<tr>

<td>TINYINT</td>

<td>byte</td>

<td>setByte</td>

<td>getByte</td>

</tr>

<tr>

<td>SMALLINT</td>

<td>short</td>

<td>setShort</td>

<td>getShort</td>

</tr>

<tr>

<td>INTEGER</td>

<td>int</td>

<td>setInt</td>

<td>getInt</td>

</tr>

<tr>

<td>BIGINT</td>

<td>long</td>

<td>setLong</td>

<td>getLong</td>

</tr>

<tr>

<td>REAL</td>

<td>float</td>

<td>setFloat</td>

<td>getFloat</td>

</tr>

<tr>

<td>FLOAT</td>

<td>float</td>

<td>setFloat</td>

<td>getFloat</td>

</tr>

<tr>

<td>DOUBLE</td>

<td>double</td>

<td>setDouble</td>

<td>getDouble</td>

</tr>

<tr>

<td>VARBINARY</td>

<td>byte[ ]</td>

<td>setBytes</td>

<td>getBytes</td>

</tr>

<tr>

<td>BINARY</td>

<td>byte[ ]</td>

<td>setBytes</td>

<td>getBytes</td>

</tr>

<tr>

<td>DATE</td>

<td>java.sql.Date</td>

<td>setDate</td>

<td>getDate</td>

</tr>

<tr>

<td>TIME</td>

<td>java.sql.Time</td>

<td>setTime</td>

<td>getTime</td>

</tr>

<tr>

<td>TIMESTAMP</td>

<td>java.sql.Timestamp</td>

<td>setTimestamp</td>

<td>getTimestamp</td>

</tr>

<tr>

<td>CLOB</td>

<td>java.sql.Clob</td>

<td>setClob</td>

<td>getClob</td>

</tr>

<tr>

<td>BLOB</td>

<td>java.sql.Blob</td>

<td>setBlob</td>

<td>getBlob</td>

</tr>

<tr>

<td>ARRAY</td>

<td>java.sql.Array</td>

<td>setARRAY</td>

<td>getARRAY</td>

</tr>

<tr>

<td>REF</td>

<td>java.sql.Ref</td>

<td>SetRef</td>

<td>getRef</td>

</tr>

<tr>

<td>STRUCT</td>

<td>java.sql.Struct</td>

<td>SetStruct</td>

<td>getStruct</td>

</tr>

</table>

# 日期和时间数据类型 #

java.sql.Date 类映射 SQL DATE 类型， java.sql.Time 类和  java.sql.Timestamp 类也分别映射 SQL TIME 数据类型和 SQL TIMESTAMP 数据类型。

以下示例显示了日期和时间类如何转换成标准的 Java 日期和时间值，并匹配成 SQL 数据类型所要求的格式。

```
import java.sql.Date;
import java.sql.Time;
import java.sql.Timestamp;
import java.util.*;

public class SqlDateTime {
   public static void main(String[] args) {
      //Get standard date and time
      java.util.Date javaDate = new java.util.Date();
      long javaTime = javaDate.getTime();
      System.out.println("The Java Date is:" + 
             javaDate.toString());

      //Get and display SQL DATE
      java.sql.Date sqlDate = new java.sql.Date(javaTime);
      System.out.println("The SQL DATE is: " + 
             sqlDate.toString());

      //Get and display SQL TIME
      java.sql.Time sqlTime = new java.sql.Time(javaTime);
      System.out.println("The SQL TIME is: " + 
             sqlTime.toString());
      //Get and display SQL TIMESTAMP
      java.sql.Timestamp sqlTimestamp =
      new java.sql.Timestamp(javaTime);
      System.out.println("The SQL TIMESTAMP is: " + 
             sqlTimestamp.toString());
     }//end main
}//end SqlDateTime
```

现在，让我们用下面的命令编译上面的代码-

```
C:\>javac JDBCExample.java
C:\>
```

当你运行 **JDBCExample** 时，它将展示下面的结果-

```
C:\>java SqlDateTime
The Java Date is:Tue Aug 18 13:46:02 GMT+04:00 2009
The SQL DATE is: 2009-08-18
The SQL TIME is: 13:46:02
The SQL TIMESTAMP is: 2009-08-18 13:46:02.828
C:\>
```

# 处理 NULL 值 #

SQL 使用 NULL 值和 Java 使用 null 是不同的概念。那么，你可以使用三种策略来处理 Java 中的 SQL NULL 值-

- 避免使用返回原始数据类型的 getXXX() 方法。
- 使用包装类的基本数据类型，并使用 ResultSet 对象的 wasNull() 方法来测试收到 getXXX() 方法返回的值是否为 null ，如果是 null ，该包装类变量则被设置为 null 。
- 使用原始数据类型和 ResultSet 对象的 wasNull() 方法来测试通过  getXXX() 方法返回的值，如果是 null ，则原始变量应设置为可接受的值来代表 NULL 。

下面是一个处理 NULL 值的示例-

```
Statement stmt = conn.createStatement( );
String sql = "SELECT id, first, last, age FROM Employees";
ResultSet rs = stmt.executeQuery(sql);

int id = rs.getInt(1);
if( rs.wasNull( ) ) {
   id = 0;
}
```