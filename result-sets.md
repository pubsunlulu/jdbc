# 结果集

SQL 语句从数据库查询中获取数据，并将数据返回到结果集中。 SELECT 语句是一种标准的方法，它从一个数据库中选择行记录，并显示在一个结果集中。 *java.sql.ResultSet* 接口表示一个数据库查询的结果集。

一个 ResultSet 对象控制一个光标指向当前行的结果集。术语“结果集”是指包含在 ResultSet 对象中的行和列的数据。
### 
ResultSet 接口的方法可细分为三类-

- **导航方法：**用于移动光标.
- **获取方法：**用于查看当前行被光标所指向的列中的数据。
- **更新方法：**用于更新当前行的列中的数据。这些更新也会更新数据库中的数据。

光标的移动基于 ResultSet 的属性。用相应的语句生成 ResultSet 对象时，同时生成 ResultSet 的属性。

JDBC 提供了连接方法通过下列创建语句来生成你所需的 ResultSet 对象：

- **createStatement(int RSType, int RSConcurrency);**
- **prepareStatement(String SQL, int RSType, int RSConcurrency);**
- **prepareCall(String sql, int RSType, int RSConcurrency);**

第一个参数表示 ResultSet 对象的类型，第二个参数是两个 ResultSet  常量之一，该常量用于判断该结果集是只读的还是可修改的。

## ResultSet 的类型

可能的 RSType 如下所示。如果你不指定 ResultSet 类型，将自动获得的值是 TYPE_FORWARD_ONLY 。

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr>
<th style="width:42%">类型</th>
<th style="width:58%">描述</th>
</tr>
<tr>
<td>ResultSet.TYPE_FORWARD_ONLY</td>
<td>光标只能在结果集中向前移动。</td>
</tr>
<tr>
<td>ResultSet.TYPE_SCROLL_INSENSITIVE</td>
<td>光标可以向前和向后移动。当结果集创建后，其他人对数据库的操作不会影响结果集的数据。</td>
</tr>
<tr>
<td>ResultSet.TYPE_SCROLL_SENSITIVE.</td>
<td>光标可以向前和向后移动。当结果集创建后，其他人对数据库的操作会影响结果集的数据。</td>
</tr>
</table>

## ResultSet 的并发性

RSConcurrency 的值如下所示，如果你不指定并发类型，将自动获得的值是 CONCUR_READ_ONLY 。

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr>
<th style="width:42%">并发性</th>
<th style="width:58%">描述</th>
</tr>
<tr>
<td>ResultSet.CONCUR_READ_ONLY</td>
<td>创建一个只读结果集，这是默认的值。</td>
</tr>
<tr>
<td>ResultSet.CONCUR_UPDATABLE</td>
<td>创建一个可修改的结果集。</td>
</tr>
</table>

到目前为止我们的示例可以如下所示，可以写成初始化一个 Statement 对象来创建一个只能前进，而且只读的 ResultSet 对象-

```
try {
   Statement stmt = conn.createStatement(
                           ResultSet.TYPE_FORWARD_ONLY,
                           ResultSet.CONCUR_READ_ONLY);
}
catch(Exception ex) {
   ....
}
finally {
   ....
}
```

## 导航结果集

在 ResultSet 接口中包括如下几种方法涉及移动光标-

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr>
<th>S.N.</th>
<th style="width:95%">方法 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td><b>public void beforeFirst() throws SQLException </b>
<p>将光标移动到第一行之前。</p>
</td>
</tr>
<tr>
<td>2</td>
<td><b>public void afterLast() throws SQLException </b>
<p>将光标移动到最后一行之后。</p>
</td>
</tr>
<tr>
<td>3</td>
<td><b>public boolean first() throws SQLException </b>
<p>将光标移动到第一行。</p>
</td>
</tr>
<tr>
<td>4</td>
<td><b>public void last() throws SQLException </b>
<p>将光标移动到最后一行。</p>
</td>
</tr>
<tr>
<td>5</td>
<td><b>public boolean absolute(int row) throws SQLException</b>
<p>将光标移动到指定的第 row 行。</p>
</td>
</tr>
<tr>
<td>6</td>
<td><b>public boolean relative(int row) throws SQLException </b>
<p>将光标移动到当前指向的位置往前或往后第 row 行的位置。</p>
</td>
</tr>
<tr>
<td>7</td>
<td><b>public boolean previous() throws SQLException </b>
<p>将光标移动到上一行，如果超过结果集的范围则返回 false 。</p>
</td>
</tr>
<tr>
<td>8</td>
<td><b>public boolean next() throws SQLException </b>
<p>将光标移动到下一行，如果是结果集的最后一行则返回 false 。</p>
</td>
</tr>
<tr>
<td>9</td>
<td><b>public int getRow() throws SQLException </b>
<p>返回当前光标指向的行数的值。</p>
</td>
</tr>
<tr>
<td>10</td>
<td><b>public void moveToInsertRow() throws SQLException </b>
<p> 将光标移动到结果集中指定的行，可以在数据库中插入新的一行。当前光标位置将被记住。</p>
</td>
</tr>
<tr>
<td>11</td>
<td><b>public void moveToCurrentRow() throws SQLException </b>
<p> 如果光标处于插入行，则将光标返回到当前行，其他情况下，这个方法不执行任何操作。</p>
</td>
</tr>
</table>

为了更好地理解，让我们研究学习**导航示例代码**。

## 查看结果集

ResultSet接口中含有几十种从当前行获取数据的方法。

每个可能的数据类型都有一个 get 方法，并且每个 get 方法有两个版本-

- 一个需要列名。
- 一个需要列的索引。

例如，如果你想查看的列包含一个int类型，你需要在 ResultSet 中调用  getInt() 方法-

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr>
<th>S.N.</th>
<th style="width:95%">方法 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td><b>public int getInt(String columnName) throws SQLException</b>
<p>返回当前行中名为 columnName 的列的 int 值。</p>
</td>
</tr>
<tr>
<td>2</td>
<td><b>public int getInt(int columnIndex) throws SQLException</b>
<p> 返回当前行中指定列的索引的 int 值。列索引从 1 开始，意味着行中的第一列是 1 ，第二列是 2 ，以此类推。</p>
</td>
</tr>
</table>

同样的，在 ResultSet 接口中还有获取八个 Java 原始类型的 get 方法，以及常见的类型，比如 java.lang.String，java.lang.Object 和  java.net.URL 。

也有用于获取 SQL 数据类型 java.sql.Date ， java.sql.Time ，  java.sql.Timestamp ， java.sql.Clob ， java.sql.Blob 中的方法。查看文档可以了解使用这些 SQL 数据类型的更多的信息。

为了更好地理解，让我们研究学习查看 - 示例代码。

## 更新的结果集

ResultSet 接口包含了一系列的更新方法，该方法用于更新结果集中的数据。

用 get 方法可以有两个更新方法来更新任一数据类型-

- 一个需要列名。
- 一个需要列的索引。

例如，要更新一个结果集的当前行的 String 列，你可以使用任一如下所示的 updateString（） 方法-

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr>
<th>S.N.</th><th style="width:95%">方法 &amp; 描述</th>
</tr>
<tr><td>1</td><td><b>public void updateString(int columnIndex, String s) throws SQLException</b>
<p>将指定列的字符串的值改为 s 。</p>
</td>
</tr>
<tr>
<td>2</td>
<td><b>public void updateString(String columnName, String s) throws SQLException</b>
<p>类似于前面的方法，不同之处在于指定的列是用名字来指定的，而不是它的索引。</p>
</td>
</tr>
</table>

八个原始数据类型都有其更新方法，比如 String ， Object ， URL ，和在 java.sql 包中的 SQL 数据类型。

更新结果集中的行将改变当前行的列中的 ResultSet 对象，而不是基础数据库中的数据。要更新数据库中一行的数据，你需要调用以下的任一方法-

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr>
<th>S.N.</th>
<th style="width:95%">方法 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td><b>public void updateRow()</b>
<p>通过更新数据库中相对应的行来更新当前行。</p>
</td>
</tr>
<tr>
<td>2</td>
<td><b>public void deleteRow()</b>
<p>从数据库中删除当前行。</p>
</td>
</tr>
<tr>
<td>3</td>
<td><b>public void refreshRow()</b>
<p>在结果集中刷新数据，以反映数据库中最新的数据变化。</p>
</td>
</tr>
<tr>
<td>4</td>
<td><b>public void cancelRowUpdates()</b>
<p>取消对当前行的任何修改。</p>
</td>
</tr>
<tr>
<td>5</td>
<td><b>public void insertRow()</b>
<p>在数据库中插入一行。本方法只有在光标指向插入行的时候才能被调用。</p>
</td>
</tr>
</table>

为了更好地理解，建议研究学习**更新 - 示例代码**。