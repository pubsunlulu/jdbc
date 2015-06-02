在开始使用 JDBC 之前，你必须如下面所示设置你的JDBC环境。我们假设你正在使用的是 Windows 平台。

# 安装 Java #
从** Java 官方网站**上安装 J2SE Development Kit 5.0 (JDK 5.0)。

请按如下所述设置环境变量-

- **JAVA\_HOME :** 该环境变量应该指向你安装的JDK目录，例如： C:\Program Files\Java\jdk1.5.0 .
- **CLASSPATH :** 该环境变量应该有适当的路径设置，例如： C:\Program Files\Java\jdk1.5.0\_20\jre\lib .
- **PATH ：** 这个环境变量应该指向 JRE bin ，例如： C:\Program Files\Java\jre1.5.0\_20\bin .
 
可能你已经设置好这些变量了，这儿告诉你如何设置是为了确保正确。

- 进入控制面板，双击系统。如果你是 Windows XP 的用户，在打开性能和维护之前，你会看到系统的图标。
- 进入高级选项卡，然后单击环境变量。
- 现在检查所有上述变量的设置是否正确。

当你安装 J2SE Development Kit 5.0 (JDK 5.0) 的时候，会自动获得了 JDBC包 **java.sql** 和 **javax.sql** 。

# 安装数据库 #

你最重要的事情当然是安装一个实际运行的数据库，你可以在该数据库里查询和修改表。

大部分的数据库都适合你用。你可以有多种选择，最常见的是-

- **MySQL 数据库：** MySQL 是一个开源数据库。你可以从MySQL官方网站上下载。我们建议你下载完整的 Windows 版本。
 此外，下载并安装 MySQL 管理器以及 MySQL 查询浏览器。这些都是基于  GUI 的工具，可以让你的开发变得更加容易。
 最后，下载并解压缩 MySQL Connector / J（MySQL JDBC驱动程序）到合适的目录。对于本教程的目的，我们假设你已经安装了驱动程序在 C:\Program Files\MySQL\mysql-connector-java-5.1.8 目录下.
 因此，设置 CLASSPATH 变量为 C:\Program Files\MySQL\mysql-connector-java-5.1.8\mysql-connector-java-5.1.8-bin.ja r。你的驱动程序版本可能会根据你的安装而有所不同。
- **PostgreSQL 数据库：** PostgreSQL 是一个开源数据库。你可以从  PostgreSQL 的官方网站下载。
  Postgres 安装包包含了一个名为 pgAdmin III 基于 GUI 的管理工具。 JDBC 驱动程序也是安装包的一部分。
- **Oracle 数据库：** Oracle 数据库是 Oracle 公司销售的商用数据库。我们假设你已经安装了必须的分发介质。
  Oracle 安装包包括一个名为 Enterprise Manager 基于 GUI 的管理工具。  JDBC 驱动程序也是安装包的一部分。

# 安装数据库驱动程序 #
最新的 JDK 包含一个 JDBC-ODBC 桥驱动程序，程序员通过使用 JDBC API 可以操作大多数开放式数据库 （ODBC） 驱动程序

现在，大部分的数据库厂商都提供与数据库安装相适应的 JDBC 驱动程序。所以，这部分你不必担心。

# 设置数据库认证 #

在本教程中，我们将使用 MySQL 数据库。当你安装上述任一数据库时，管理员 ID 设置为 **root** ，并给出规定设置选择的密码。

使用 root 账号和密码，你可以创建另一个账号和密码，或者你可以在  JDBC 的程序中直接使用 root 账户和密码。

各种数据库的操作，比如数据库的创建和删除，都需要管理员的账户和密码。

对于 JDBC 教程的其余部分，在 MySQL 数据库中我们将使用  **username** 作为账户 **password** 作为密码。

如果你没有足够的权限创建新用户，那么你可以要求你的数据库管理员 （DBA） 创建一个用户账户和密码给你。

# 创建数据库 #

要创建 **EMP** 数据库，请使用以下步骤-

## 步骤1 ##

通过以下步骤打开一个**命令提示符**并切换到安装目录下-

```
C:\>
C:\>cd Program Files\MySQL\bin
C:\Program Files\MySQL\bin>
```

**注意： mysqld.exe** 的路径会根据你系统上安装的路径而可能会有所不同。你也可以查看如何启动和停止你的数据库服务器文档。

## 步骤2 ##

通过以下步骤来启动数据库服务器，如果该数据库服务器没有运行。

```
C:\Program Files\MySQL\bin>mysqld
C:\Program Files\MySQL\bin>
```
## 步骤3 ##

通过以下步骤来创建 **EMP** 数据库-

```
C:\Program Files\MySQL\bin> mysqladmin create EMP -u root -p
Enter password: ********
C:\Program Files\MySQL\bin>
```

# 创建表格 #

通过以下步骤在 EMP 数据库中创建 **Employees** 表-

## 步骤1 ##

通过以下步骤打开一个**命令提示符**并切换到安装目录下-

```
C:\>
C:\>cd Program Files\MySQL\bin
C:\Program Files\MySQL\bin>
```


## 步骤2 ##

通过以下步骤登录到数据库。

```
C:\Program Files\MySQL\bin>mysql -u root -p
Enter password: ********
mysql>
```

## 步骤3 ##

通过以下命令来创建 **Employee** 表-

```
mysql> use EMP;
mysql> create table Employees
    -> (
    -> id int not null,
    -> age int not null,
    -> first varchar (255),
    -> last varchar (255)
    -> );
Query OK, 0 rows affected (0.08 sec)
mysql>
```

## 创建数据记录 ##

最后，通过以下步骤你可以在 Employee 表中创建几条记录-

```
mysql> INSERT INTO Employees VALUES (100, 18, 'Zara', 'Ali');
Query OK, 1 row affected (0.05 sec)

mysql> INSERT INTO Employees VALUES (101, 25, 'Mahnaz', 'Fatma');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO Employees VALUES (102, 30, 'Zaid', 'Khan');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO Employees VALUES (103, 28, 'Sumit', 'Mittal');
Query OK, 1 row affected (0.00 sec)

mysql>
```

通过研究学习 **MySQL 教程**，可以完整的了解 MySQL 数据库。

现在，你就可以尝试使用 JDBC 。下一章将为你提供了一个简单的 JDBC 编程示例。
