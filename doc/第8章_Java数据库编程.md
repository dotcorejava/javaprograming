# 第8章 Java数据库编程

## JDBC 技术 
DBC是一套允许Java同一个SQL数据库对话的程序设计接口。JDBC经常被认为是Java Database Connectivity的缩写。事实上,JDBC是一个产品的商标名,由一组用Java编程语言编写的类和接口组成。

1. 与特定的数据库进行连接。
2. 向数据库发送SQL语句,实现对数据库的特定操作。
3. 对数据库返回的结果进行处理。

### JDBC 驱动程序
1. JDBC-ODBC桥  
JDBC-ODBC Bridge是一种JDBC驱动程序,由JavaSoft公司提出、INTERSOIV公司开发。它充分发挥了ODBC支持大量数据源的优势。JDBC利用JDBC-ODBC Bridge,通过ODBC操作数据库。因为ODBC早已成为数据库访问的业界标准,所以JDBC-ODBC Bridge的出现更多的是出于市场因素的考虑。它最直接的缺点就是依赖ODBC,这样ODBC的局限性将存在于使用JDBC-ODBC Bridge作为驱动的程序中。

2. 本地API部分Java驱动程序  
本地API部分Java驱动程序(Java to Native API)是利用客户机上的本地代码库与数据库直接通信。因为这类驱动程序必须使用本地库,所以这些库必须实现安装在客户机上。不过大多数的数据库供应商都为其产品提供了该类型的驱动程序。

3. **JDBC-Net纯Java驱动程序**  
JDBC-Net纯Java驱动程序是面向数据库中间件的纯Java驱动程序,JDBC调用被转换成一种中间件厂商的协议,中间件再把这些调用转换到数据库API。这种驱动程序比较灵活,能够发布到网上,常被用在三层网络解决方案中。这种驱动程序通常由一些与数据库产品无关的公司开发。

4. **纯Java的驱动程序**  
纯Java的驱动程序通过本地协议直接与数据库引擎相连接。配合合适的通信协议,这种驱动程序也可以用于Internet。由于数据库引擎和客户机之间没有本地代码层或中间软件,因此具有明显的性能优势。

>早在JDBC还未成形之时,Microsoft的ODBC已经成为数据库访问的业界标准,是使用非常广泛的访问关系数据库的编程接口。它几乎能在所有平台上连接所有的数据库。但ODBC不适合直接在Java中使用,因为其使用C语言接口。从Java调用本地C代码在安全性、实现、坚固性和程序的自动移植性方面都有许多缺点。

## 结构化查询语言 SQL
结构化查询语言(Structured Query Language,SQL)是一种数据库查询和程序设计语言,用于存取数据及查询、更新和管理关系数据库系统,同时也是数据库脚本文件的扩展名。  

SQL语言集数据查询、数据操纵、数据定义和数据控制功能于一体。SQL语言具有以下特点:
1. 综合统一。
2. 高度非过程化。
3. 面向集合的操作方式。
4. 以同一种语法结构提供两种使

## JDBC 基本操作

|序号|类/接口|功能说明|
|---|---|---|
|1|DriverManager 类|用于加载和卸载各种驱动程序并建立与数据库的连接|
|2|Connection 类|此接口表示与数据库的连接|
|3|Statement 接口|此接口用于执行 SQL 语句宾|
|4|ResultSet 接口|此接口表示查询出来的数据库数据结果集|
|5|PreparedStatement 接口|此接口用于执行预编译的 SQL 语句|
|6|Date 类|包含将 SQL 日期格式转换为 Java 日期格式的各种方法|

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class HelloWorld {
        public static void main(String[] args) throws SQLException {
                addUser();
                queryUsers();
        }

        private static void addUser() throws SQLException {
                try {
                        // DriverManager.registerDriver((Driver)Class.forName("com.mysql.jdbc.Driver").newInstance());
                        DriverManager.registerDriver(DriverManager.getDriver("jdbc:mysql://127.0.0.1:1306/jdbcd"));
                } catch (SQLException e) {
                        e.printStackTrace();
                        return;
                }

                try (Connection connection = DriverManager.getConnection(
                                "jdbc:mysql://127.0.0.1:1306/jdbcd?useUnicode=true&characterEncoding=utf8&autoReconnect=true",
                                "root", "dmtest")) {
                        try (Statement statement = connection.createStatement()) {
                                statement.executeUpdate("INSERT INTO users (name,age,gender) VALUES ('李四旺',18,1)");
                        }
                }
        }

        private static void queryUsers() throws SQLException {
                try {
                        // DriverManager.registerDriver((Driver)Class.forName("com.mysql.jdbc.Driver").newInstance());
                        DriverManager.registerDriver(DriverManager.getDriver("jdbc:mysql://127.0.0.1:1306/jdbcd"));
                } catch (SQLException e) {
                        e.printStackTrace();
                        return;
                }

                try (Connection connection = DriverManager.getConnection(
                                "jdbc:mysql://127.0.0.1:1306/jdbcd?useUnicode=true&characterEncoding=utf8&autoReconnect=true",
                                "root", "dmtest")) {
                        try (Statement statement = connection.createStatement()) {
                                ResultSet resultSet = statement.executeQuery("SELECT id,name,age,gender FROM users");
                                while ((resultSet.next())) {
                                        System.out.printf("{id:%d;name:%s,age:%d,gender:%b},%n", resultSet.getInt("id"),
                                                        resultSet.getString("name"), resultSet.getInt("age"),
                                                        resultSet.getBoolean("gender"));
                                }
                        }
                }
        }
}
```

## JDBC 高级操作
- PreparedStatemen 接口进行预处理操作
- CallableStatement 接口调用数据中的存储过程

### PreparedStatemen 接口
```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class HelloWorld {
        public static void main(String[] args) throws SQLException {
                addUser();
        }

        private static void addUser() throws SQLException {
                try {
                        // DriverManager.registerDriver((Driver)Class.forName("com.mysql.jdbc.Driver").newInstance());
                        DriverManager.registerDriver(DriverManager.getDriver("jdbc:mysql://127.0.0.1:1306/jdbcd"));
                } catch (SQLException e) {
                        e.printStackTrace();
                        return;
                }

                try (Connection connection = DriverManager.getConnection(
                                "jdbc:mysql://127.0.0.1:1306/jdbcd?useUnicode=true&characterEncoding=utf8&autoReconnect=true",
                                "root", "dmtest")) {
                        String queryString = "INSERT INTO users (name,age,gender) VALUES (?,?,?)";
                        try (PreparedStatement preparedStatement = connection.prepareStatement(queryString)) {
                                // try (Statement statement = connection.createStatement()) {
                                // statement.executeUpdate("INSERT INTO users (name,age,gender) VALUES
                                // ('李四旺',18,0)");
                                preparedStatement.setString(1, "张开山");
                                preparedStatement.setInt(2, 23);
                                preparedStatement.setBoolean(3, true);
                                preparedStatement.executeUpdate();
                        }
                }
        }
}
```

### CallableStatement 接口
```
DELIMITER $$
CREATE PROCEDURE add_user 
(IN name VARCHAR(10), IN age INT, IN gender TINYINT)
BEGIN
INSERT INTO users (users.name,users.age,users.gender) VALUES (name,age,gender);
END$$
```

```
CALL add_user('kkzz',19,1);
```

```
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class HelloWorld {
        public static void main(String[] args) throws SQLException {
                addUser();
        }

        private static void addUser() throws SQLException {
                try {
                        // DriverManager.registerDriver((Driver)Class.forName("com.mysql.jdbc.Driver").newInstance());
                        DriverManager.registerDriver(DriverManager.getDriver("jdbc:mysql://127.0.0.1:1306/jdbcd"));
                } catch (SQLException e) {
                        e.printStackTrace();
                        return;
                }

                try (Connection connection = DriverManager.getConnection(
                                "jdbc:mysql://127.0.0.1:1306/jdbcd?useUnicode=true&characterEncoding=utf8&autoReconnect=true",
                                "root", "dmtest")) {
                        String queryString = "{call add_user(?,?,?)}";
                        try (CallableStatement callableStatement = connection.prepareCall(queryString)) {
                                callableStatement.setString(1, "遍地莲");
                                callableStatement.setInt(2, 23);
                                callableStatement.setInt(3, 0);
                                callableStatement.executeUpdate();
                        }
                }
        }
}
```
**CallableStatement对象的创建形式有三种:**
1. 不带参数的存储过程。  
例如:CallableStatement cs=conn.preparedCall(“{call存储过程名()}”);
2. 传入IN参数的存储过程。  
例如:CallableStatement cs=conn.preparedCall(“{call存储过程名(?,?,?)}”);
需要使用setXXX()方法为相应的占位符赋值。
3. 传入IN或OUT参数的存储过程。  
例如:CallableStatement cs=conn.preparedCall(“{?=call存储过程名(?,?,?)}”);
需要使用getXXX()方法获得输出参数,setXXX()方法为相应的占位符赋值。

## 事务处理
事务是用户定义的一个数据库操作序列,这些操作要么全做,要么全不做,是一个不可分割的工作单位。在关系数据库中,一个事务可以是一条SQL语句、一组SQL语句或整个程序。事务具有4个特性:原子性、一致性、隔离性和持续性。  

在JDBC中默认是自动提交的。自动提交的含义是把每一条SQL语句作为一个事务,而更多的时候事务是指一组SQL语句。在JDBC中,如果想进行事务处理,也需要按照指定的步骤完成。
1. 取消Connection中设置的自动提交方式“conn.setAutoCommit(false);”。
2. 如果批处理操作成功,就执行提交事务“conn.commit();”。
3. 如果操作失败,就会引发异常,在异常处理中让事务回滚“conn.rollback();”。

## 练习
### 填空题
1. JDBC为开发人员提供了一个标准的API,它由一组用Java编程语言编写的类和__组成。
2. SQL语言之所以能成为国际标准,是因为其集__、__、__和__功能于一体。
3. SQL中提供了5种聚集函数,分别是__、__ 、__、__和 __。
4. 调用方法Class.forName()将显式地将驱动程序添加到__的属性jdbc.drivers中。
5. 事务具有4个特性:原子性、__、隔离性和 __。

### 选择题
1. 下列不属于SQL聚集函数的是__。  
A. SUM  
B. AVG  
C. COUNT  
D. NVL
2. Statement接口中的哪个方法可以用于执行数据定义语言__。  
A. execute  
B. addBatch  
C. executeUpdate  
D. executeQuery  
3. Connection接口中的哪个方法用于获取DatabaseMetaData接口__。  
A. getMetaData  
B. createStatement  
C. prepareStatement  
D. prepareCall
4. 下列哪个接口用于获取元数据__。  
A. Statement  
B. PreparedStatement  
C. Connection  
D. DatabaseMetaData
5. Connection接口中的哪个方法用于设置事务自动提交__。  
A. commit  
B. setAutoCommit  
C. getAutoCommit  
D. rollback

### 问答题
1. 列举并说明JDBC驱动的4种类型。
2. 简述JDBC和ODBC的关系和异同。

### 编程练习
编写一个java应用程序,添加、修改和删除Student表中的记录。

|列名称|数据类型|
|---|---|
|Name|Varchar(20)|
|Rollno|Numeric|
|Course|Varchar(20)|