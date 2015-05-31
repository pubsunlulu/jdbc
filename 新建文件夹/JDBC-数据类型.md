JDBC驱动程序在将Java数据类型发送到数据库之前，会将其转换为相应的JDBC类型。对于大多数数据类型都采用了默认的映射关系。例如，一个Java int数据类型转换为SQL INTEGER。通过默认的映射关系来提供驱动程序之间的一致性。

当你调用PreparedStatement中的setXXX()方法或CallableStatement对象或ResultSet.updateXXX()方法时，Java数据类型会转换为默认的JDBC数据类型，如下表概述。

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

JDBC3.0增强了对BLOB，CLOB，ARRAY和REF数据类型的支持。 ResultSet对象现在有UPDATEBLOB()，updateCLOB()，updateArray()，和updateRef()方法，通过这些方法你可以直接操作服务器上的相应数据。

你能用setXXX()方法和updateXXX()方法将Java类型转换为特定的JDBC数据类型。你能用setObject()方法和updateObject()方法将绝大部分的Java类型映射到JDBC数据类型。

ResultSet对象为任一数据类型提供相应的getXXX()方法，该方法可以获取任一数据类型的列值。上述任一方法的使用需要列名或它的顺序位置。

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

java.sql.Date类映射SQL DATE类型，java.sql.Time类和java.sql.Timestamp类也分别映射SQL TIME数据类型和SQL TIMESTAMP数据类型。

以下示例显示了日期和时间类如何转换成标准的Java日期和时间值，并匹配成SQL数据类型所要求的格式。

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

当你运行 **JDBCExample**时，它将展示下面的结果-

```
C:\>java SqlDateTime
The Java Date is:Tue Aug 18 13:46:02 GMT+04:00 2009
The SQL DATE is: 2009-08-18
The SQL TIME is: 13:46:02
The SQL TIMESTAMP is: 2009-08-18 13:46:02.828
C:\>
```

# 处理NULL值 #

SQL使用NULL值和Java使用null是不同的概念。那么，你可以使用三种策略来处理Java中的SQL NULL值-

- 避免使用返回原始数据类型的getXXX()方法。
- 使用包装类的基本数据类型，并使用ResultSet对象的wasNull()方法来测试收到getXXX()方法返回的值是否为null，如果是null，该包装类变量则被设置为null。
- 使用原始数据类型和ResultSet对象的wasNull()方法来测试通过getXXX()方法返回的值，如果是null，则原始变量应设置为可接受的值来代表NULL。

下面是一个处理NULL值的示例-

```
Statement stmt = conn.createStatement( );
String sql = "SELECT id, first, last, age FROM Employees";
ResultSet rs = stmt.executeQuery(sql);

int id = rs.getInt(1);
if( rs.wasNull( ) ) {
   id = 0;
}
```