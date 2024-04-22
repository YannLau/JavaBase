P821---P857

[toc]

# JDBC概述

> 基本介绍
>
> 1. JDBC为访问不同的数据库提供了统一的接口，为使用者屏蔽了细节问题。
>
> 2. Java程序员使用JDBC，可以连接任何提供了JDBC驱区动程序的数据库系统，从而完成对数据库的各种操作。
> 3. JDBC的基本原理图［重要!]

![image-20240303下午103143321](/Users/yannlau/Library/Application Support/typora-user-images/image-20240303下午103143321.png)

> JDBC API是一系列的接口，它统一和规范了应用程序与数据库的连接、执行SQL语句，并到得到返回结果等各类操作，相关类和接口在java.sql 与 javax.sql 包中

![image-20240304上午92547884](/Users/yannlau/Library/Application Support/typora-user-images/image-20240304上午92547884.png)

> • JDBC带来的好处
> 2. JDBC带来的好处（示意图 )
> 2. 说明：JDBC是Java提供一套用于数据库操作的接口API,Java
> 程序员只需要面向这套接口编程即可。不同的数据库厂商，需要针对这套接口，提供不同实现。

# JDBC快速入门

> JDBC程序编写步骤
>
> 1. 注册驱动 - 加载Driver类
> 2. 获取连接 - 得到Connection
> 3. 执行增删改查 - 发送SQL给mysql
> 4. 释放资源 - 关闭相关连接

```mysql
-- 创建测试表 演员表
use hsp_db02;
create table actor(  -- 演员表
	id int primary key auto_increment,
	name varchar(32) not null default '',
	sex char(1) not null default '女',
	borndate datetime,
	phone varchar(12));
```

![image-20240304下午22047630](/Users/yannlau/Library/Application Support/typora-user-images/image-20240304下午22047630.png)

```java
import java.sql.Connection;
import java.sql.Driver;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class Jdbc01 {
    public static void main(String[] args) throws SQLException {

        //前置工作:在项目下创建一个文件夹比如libs
        // 将 mysql.jar 拷贝到该目录下,点击add to library 加入到项目中

        //1. 注册驱动
        Driver driver = new com.mysql.cj.jdbc.Driver();

        //2. 得到连接
        // 老师解读
        //（1） jdbc:mysql:// 规定好表示协议，通过jdbc的方式连接mysql
        //（2） Localhost 主机，可以是ip地址
        // (3) 3306 表示mysql监听的端口
        // (4) db01 连接到mysql dbms 的哪个数据库
        // (5) mysql的连接的本质就是前面学过的socket连接
        String url = "jdbc:mysql://localhost:3306/db01";
        // 将用户名和密码放入到Properties对象
        Properties properties = new Properties();
        //说明 user 和 password 是规定好,后面的值根据实际情况写
        properties.setProperty("user", "root");//用户
        properties.setProperty("password", "sjywza123.");//密码
        Connection connect = driver.connect(url, properties);

        //3. 执行sql
        String sql = "insert into actor values(null,'刘德华','男','1970-11-11','110')";
        //statement 用于执行静态SQL语句并返回其生成的结果的对象
        Statement statement = connect.createStatement();
        int rows = statement.executeUpdate(sql);//如果是dml语句,返回的就是影响行数
        System.out.println(rows > 0 ? "成功" : "失败");
      
        //4. 关闭连接资源
        statement.close();
        connect.close();
    }
}
```

# 数据库连接方式

> ```java
> //方式1 会直接使用 com.mysqljdbc.Driver()，属于静态加载，灵活性差，依赖强
> 
> import java.io.FileInputStream;
> import java.io.FileNotFoundException;
> import java.io.IOException;
> import java.sql.Connection;
> import java.sql.Driver;
> import java.sql.DriverManager;
> import java.sql.SQLException;
> import java.util.Properties;
> 
> public class JdbcConn {
>     //方式一
>     public void connect01() throws SQLException {
>         Driver driver = new com.mysql.cj.jdbc.Driver();
>         String url = "jdbc:mysql://localhost:3306/db01";
>         // 将用户名和密码放入到Properties对象
>         Properties properties = new Properties();
>         //说明 user 和 password 是规定好,后面的值根据实际情况写
>         properties.setProperty("user", "root");//用户
>         properties.setProperty("password", "sjywza123.");//密码
>         Connection connect = driver.connect(url, properties);
>         System.out.println("方式一=" + connect);
>     }
> 
>     //方式2
>     public void connect02() throws ClassNotFoundException, InstantiationException, IllegalAccessException, SQLException {
>         //使用反射加载Driver类,动态加载,更加的灵活,减少依赖性
>         Class<?> aClass = Class.forName("com.mysql.cj.jdbc.Driver");
>         Driver driver = (Driver) aClass.newInstance();
>         String url = "jdbc:mysql://localhost:3306/db01";
>         // 将用户名和密码放入到Properties对象
>         Properties properties = new Properties();
>         //说明 user 和 password 是规定好,后面的值根据实际情况写
>         properties.setProperty("user", "root");//用户
>         properties.setProperty("password", "sjywza123.");//密码
>         Connection connect = driver.connect(url, properties);
>         System.out.println("方式2=" + connect);
>     }
> 
>     //方式3 使用DriverManager替代driver进行统一管理
>     public void connect03() throws ClassNotFoundException, InstantiationException, IllegalAccessException, SQLException {
>         // 使用DriverManager替换Driver
>         Class<?> clazz = Class.forName("com.mysql.jdbc.Driver");
>         Driver driver = (Driver) clazz.newInstance();
>         String url = "jdbc:mysql://localhost:3306/jdbc_db";
>         String user = "root";
>         String password = "hsp";
> 
>         DriverManager.registerDriver(driver);//注册Driver驱动
>         Connection conn = DriverManager.getConnection(url, user, password);
>         System.out.println("方式3=" + conn);
>     }
> 
>     //方式4
>     //这种方式获取连接是使用的最多的!推荐使用
>     public void connect04() throws ClassNotFoundException, SQLException {
>         // 使用 Class.forName 自动完成注册驱动，简化代码 =>分析源码
>         // 在加载Driver类时,完成注册
>         /*
>             源码:1. 静态代码块,在类加载时,会执行一次.
>                 2. DriverManager.registerDriver(new Driver());
>                 3. 因此注册driver的工作已经完成
>             static {
>                 try {
>                     DriverManager.registerDriver(new Driver());
>                 } catch (SQLException var1) {
>                     throw new RuntimeException("Can't register driver!");
>                 }
>             }
>          */
>         Class.forName("com.mysqljdbc.Driver");
>         String url = "jdbc:mysql://localhost:3306/jdbc_db";
>         String user = "root";
>         String password = "hsp";
>         Connection conn = DriverManager.getConnection(url, user, password);
>         System.out.println("方式4=" + conn);
>     }
> // 老韩提示：
> //1. mysql驱动5.1.6可以无需Class.forName("com.mysql.jdbc.Driver");
> //2.从jdk1.5以后使用了jdbc4，不再需要显示调用class.forName()注册驱动而是自动调用驱动
> //   jar包下META-INF\services \java.sql.Driver文本中的类名称去注册
> //3. 建议还是写上CLass.forName（“com.mysql.jdbc.Driver"），更加明确
> 
> 
>     //方式5,在方式4的基础上改进,使用配置文件，连接数据库更灵活
>     public void connect05() throws IOException, ClassNotFoundException, SQLException {
>         //通过properties对象获取配置文件的信息
>         Properties properties = new Properties();
>         properties.load(new FileInputStream("src\\mysql.properties"));
>         //获取相关值
>         String user = properties.getProperty("user");
>         String password = properties.getProperty("password");
>         String driver = properties.getProperty("driver");
>         String url = properties.getProperty("url");
> 
>         Class.forName("com.mysql.jdbc.Driver"); //建议写上
>         Connection connection = DriverManager.getConnection(url, user, password);
> 
>         System.out.println("方式5 = " + connection);
>     }
> }
> ```
>
> ```Properties
> user=root
> password=hsp
> url=jdbc:mysql://localhost: 3306/hsp_db02
> driver=com.mysql.jdbc.Driver
> ```

# ResultSet

• 基本介绍
1. 表示数据库结果集的数据表，通常通过执行查询数据库的语句生成
2. ResultSet对象保持一个光标指向其当前的数据行。最初，光标位于第一行之前
3. next方法将光标移动到下一行，并且由于在ResultSet对象中没有更多行时返回
    false，因此可以在while循环中使用循环来遍历结果集

![image-20240304下午73816159](/Users/yannlau/Library/Application Support/typora-user-images/image-20240304下午73816159.png)

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;

public class ResultSet_ {
    public static void main(String[] args) throws IOException, SQLException, ClassNotFoundException {
        //通过properties对象获取配置文件的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("/Users/yannlau/IdeaProjects/jdbc_learning/src/mysql.properties"));
        //获取相关值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        //1. 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver"); //建议写上
        //2. 得到连接
        Connection connection = DriverManager.getConnection(url, user, password);
        //3. 得到statement
        Statement statement = connection.createStatement();
        //4. 组织sql
        String sql = "select id,name,sex,borndate from actor";
        ResultSet resultSet = statement.executeQuery(sql);
        //5. 使用while取出数据
        while (resultSet.next()) { //让光标向后移动，如果没有更多行，则返回false
            int id = resultSet.getInt(1);//获取该行的第1列
            String name = resultSet.getString(2);//获取该行的第2列
            String sex = resultSet.getString(3);
            Date date = resultSet.getDate(4);
            System.out.println(id + "\t" + name + "\t" + sex + "\t" + date);
        }
        /*
            老韩阅读debug代码 resultSet对象的结构
         */
        //6. 关闭连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

# Statement 

• 基本介绍
1. Statement对象 用于执行静态SQL语句并返回其生成的结果的对象
2. 在连接按建立后，需要对数据库进行访问，执行 命名或是SQL语句，可以通过
    • Statement ［存在SQL注入］
    • PreparedStatement［预处理］
    • CallableStatement［存储过程］
3. Statement对象执行SQL 语句，存在SQL注入风险
4. SQL 注入是利用某些系统没有对用户输入的数据进行充分的检查，而在用户输
    入数据中注入非法的SQL语句段或命令，恶意攻击数据库。sql_injection.sql
5. 要防范 SQL 注入，只要用 PreparedStatement（从Statement扩展而来）取代Statement 就可以了

```mysql
-- 演示sql注入
-- 创建一张表
create table admin( -- 管理员表
  name varchar(32) not null unique,
  pwd varchar(32) not null default '') CHARACTER set utf8;
  
-- 添加数据
insert into admin values('tom','123');
-- 查找某个管理员是否存在
select * from admin where name = 'tom' and pwd = '123~';

-- SQL
-- 输入用户名为   1' or
-- 输入密码 为   or '1'= '1

select * from admin
	where name = 'tom' and pwd = '123';
变为
select * from admin
	where name = '1' or' and pwd = 'or '1'='1'
```

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;
import java.util.Scanner;

public class Statement_ {
    public static void main(String[] args) throws IOException, ClassNotFoundException, SQLException {
        //让用户输入管理员名和密码
        String admin_name = "";
        String admin_pwd = "";
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入管理员的名字:");
        admin_name = scanner.nextLine();
        System.out.println("请输入密码");
        admin_pwd = scanner.nextLine();

        //通过properties对象获取配置文件的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("/Users/yannlau/IdeaProjects/jdbc_learning/src/mysql.properties"));
        //获取相关值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        //1. 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver"); //建议写上
        //2. 得到连接
        Connection connection = DriverManager.getConnection(url, user, password);
        //3. 得到statement
        Statement statement = connection.createStatement();
        //4. 组织sql
        String sql = "select name , pwd from admin where name ='"
                + admin_name + "'and pwd = '" + admin_pwd + "'";
        ResultSet resultSet = statement.executeQuery(sql);
        if(resultSet.next()){ //如果查询到一条记录则说明该管理存在
            System.out.println("恭喜,登陆成功");
        }else{
            System.out.println("登录失败!");
        }
    }
}

请输入管理员的名字:
1' or
请输入密码
or '1'= '1
恭喜,登陆成功
```

# PreparedStatement

1. PreparedStatement 执行的 SQL 语句中的参数用问号（?）来表示，调用
    PreparedStatement 对象的 setXxx() 方法来设置这些参数.
2. setXxx()方法有两个参数，第一个参数是要设置的SQL 语句中的参数的索引（从1开始），第二个是设置的 SQL语句中的参数的值
3. 调用 executeQueryl），返回 ResultSet 对象
4. 调用 executeUpdate()：执行更新，包括增、删、修改


> 预处理的好处
>
> 1. 不再使用＋拼接sq|语句，减少语法错误
>
> 2. 有效的解决了sql注入问题！
> 3. 大大减少了编译次数，效率较高

```java
import java.io.FileInputStream;
import java.sql.*;
import java.util.Properties;
import java.util.Scanner;

public class PreparedStatement_ {

    public static void main(String[] args) throws Exception {
        {
            //看看PreparedStatement类图
            //让用户输入管理员名和密码
            String admin_name = "";
            String admin_pwd = "";
            Scanner scanner = new Scanner(System.in);
            System.out.println("请输入管理员的名字:");
            admin_name = scanner.nextLine();
            System.out.println("请输入密码");
            admin_pwd = scanner.nextLine();

            //通过properties对象获取配置文件的信息
            Properties properties = new Properties();
            properties.load(new FileInputStream("/Users/yannlau/IdeaProjects/jdbc_learning/src/mysql.properties"));
            //获取相关值
            String user = properties.getProperty("user");
            String password = properties.getProperty("password");
            String driver = properties.getProperty("driver");
            String url = properties.getProperty("url");
            //1. 注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver"); //建议写上
            //2. 得到连接
            Connection connection = DriverManager.getConnection(url, user, password);
            //3.1 组织sql,sql语句的问号?相当于占位符
            String sql = "select name,pwd from admin where name =? and pwd = ?";
            //3.2 preparedStatement对象实现了PreparedStatement接口的实现类的对象
            PreparedStatement preparedStatement = connection.prepareStatement(sql);
            //3.3 给?赋值
            preparedStatement.setString(1, admin_name);
            preparedStatement.setString(2, admin_pwd);

            //4. 执行select语句使用 executeQuery
            // 如果执行的是 dml(update,insert,delete) executeUpdate()
            // 这里执行 executeQuery,()内不要再写sql
            ResultSet resultSet = preparedStatement.executeQuery();
            if (resultSet.next()) { //如果查询到一条记录则说明该管理存在
                System.out.println("恭喜,登陆成功");
            } else {
                System.out.println("登录失败!");
            }
            //关闭连接
            resultSet.close();
            preparedStatement.close();
            connection.close();
        }
    }
}
```

```java
import java.io.FileInputStream;
        import java.sql.Connection;
        import java.sql.DriverManager;
        import java.sql.PreparedStatement;
        import java.sql.ResultSet;
        import java.util.Properties;
        import java.util.Scanner;

public class PreparedStatement_dll {
    public static void main(String[] args) throws Exception {
//看看PreparedStatement类图
        //让用户输入管理员名和密码
        String admin_name = "";
        String admin_pwd = "";
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入管理员的名字:");
        admin_name = scanner.nextLine();
        System.out.println("请输入密码");
        admin_pwd = scanner.nextLine();

        //通过properties对象获取配置文件的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("/Users/yannlau/IdeaProjects/jdbc_learning/src/mysql.properties"));
        //获取相关值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        //1. 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver"); //建议写上
        //2. 得到连接
        Connection connection = DriverManager.getConnection(url, user, password);
        //3.1 组织sql,sql语句的问号?相当于占位符
        String sql = "insert into admin values(?,?)";
        //3.2 preparedStatement对象实现了PreparedStatement接口的实现类的对象
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        //3.3 给?赋值
        preparedStatement.setString(1, admin_name);
        preparedStatement.setString(2, admin_pwd);

        //4. 执行select语句使用 executeUpdate
        int rows = preparedStatement.executeUpdate();
        System.out.println(rows > 0 ? "执行成功!" : "执行失败!");
        //关闭连接
        preparedStatement.close();
        connection.close();
    }
}
```

# JDBC API

![image-20240305下午90217726](/Users/yannlau/Library/Application Support/typora-user-images/image-20240305下午90217726.png)

![image-20240305下午90703047](/Users/yannlau/Library/Application Support/typora-user-images/image-20240305下午90703047.png)

# 封装JDBCUtils

- 说明

  - 在jdbc 操作中，获取连接和释放资源 是经常使用到，

  - 可以将其封装JDBC连接的工具类 JDBCUtils

![image-20240305下午91818645](/Users/yannlau/Library/Application Support/typora-user-images/image-20240305下午91818645.png)

```mysql
import java.io.FileInputStream;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;

public class JDBCUtils {
    //定义相关的属性(4个) 因为只需要一份,因此我们做成static
    private static String user;//用户名
    private static String password;//密码
    private static String url;//url
    private static String driver;//驱动名

    //在static代码块去初始化
    static {
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("/Users/yannlau/IdeaProjects/jdbc_learning/src/mysql.properties"));
            //读取相关的属性值
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            url = properties.getProperty("url");
            driver = properties.getProperty("driver");
        } catch (IOException e) {
            //在实际开发中，我们可以这样处理
            //1．将编译异常转成 运行异常
            //2.调用者可以选择捕获该异常，也可以选择默认处理该异常，比较方便。
            throw new RuntimeException(e);
        }
    }

    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(url, user, password);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    //关闭相关资源
    /*
        1. ResultSet 结果集
        2. Statement 或者 PreparedStatement
        3. Connection
        4. 如果需要关闭资源,就传入对象,否则传入null
     */
    public static void close(ResultSet set, Statement statement, Connection connection) {
        //判断是否为空null
        try {
            if (set != null) {
                set.close();
            }
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        } catch (SQLException e) {
            //将编译异常转成运行异常抛出
            throw new RuntimeException(e);
        }
    }
}
```

```java
import com.lypedu.jdbc.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

//test dml语句
public class JDBCUtils_Use {
    public static void main(String[] args) {
        //1.得到链接
        Connection connection = null;
        //2. 组织一个sql
        String sql = "update actor set name = ? where id = ?";
        // 测试delete和insert 自己玩
        PreparedStatement preparedStatement = null;
        //3. 创建一个PreparedStatement对象
        try {
            connection = JDBCUtils.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            //给占位符赋值
            preparedStatement.setString(1, "周星驰");
            preparedStatement.setInt(2, 1);
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(null, preparedStatement, connection);
        }
    }
}
```

```mysql
import java.sql.*;

public class JDBCUtils_select {
    public static void main(String[] args) {
        //1.得到链接
        Connection connection = null;
        //2. 组织一个sql
        String sql = "select * from actor";
        PreparedStatement preparedStatement = null;
        ResultSet set = null;
        //3. 创建一个PreparedStatement对象
        try {
            connection = JDBCUtils.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            //执行得到结果集
            set = preparedStatement.executeQuery();
            //遍历该结果集
            while(set.next()){
                int id = set.getInt("id");
                String name = set.getString("name");
                String sex = set.getString("sex");
                Date borndate = set.getDate("borndate");
                String phone = set.getString("phone");
                System.out.println(id+"\t"+name+"\t"+sex+"\t"+borndate+"\t"+phone);
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(set, preparedStatement, connection);
        }
    }
}
```

# JDBC 事务

- 基本介绍

1. JDBC程序中当一个Connection对象创建时，默认情况下是自动提交事务：每次执行一个 SQL语句时，如果执行成功，就会向数据库自动提交，而不能回滚。
2. JDBC程序中为了让多个 SQL语句作为一个整体执行，需要<u>***使用事务***</u>
3. 调用 Connection 的 setAutoCommit（false）可以取消自动提交事务
4. 在所有的SQL 语句都成功执行后，调用commit（）；方法提交事务
5. 在其中某个操作失败或出现异常时，调用 rollbackO：方法回滚事务

```mysql
CREATE TABLE ACCOUNT (
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(32) NOT NULL DEFAULT '',
balance DOUBLE NOT NULL DEFAULT 0) CHARACTER SET utf8;

INSERT INTO ACCOUNT VALUES(NULL,'马云',3000);
INSERT INTO ACCOUNT VALUES(NULL,'马化腾',10000);

select * from account;
```

```java
public class Transaction_ {
    //没有使用事务的,
    @Test
    public void noTransaction() {
        //操作转账业务
        //1.得到链接
        Connection connection = null;
        //2. 组织一个sql
        String sql1 = "update account set balance = balance - 100 where id = 1";
        String sql2 = "update account set balance = balance + 100 where id = 2";
        PreparedStatement preparedStatement = null;
        //3. 创建一个PreparedStatement对象
        try {
            connection = JDBCUtils.getConnection();
            preparedStatement = connection.prepareStatement(sql1);
            preparedStatement.executeUpdate();//执行第一条sql语句
            int i = 1 / 0;
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();//执行第二条sql语句
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(null, preparedStatement, connection);
        }
    }
    @Test
    public void useTransaction() {
        //操作转账业务
        //1.得到链接
        Connection connection = null;
        //2. 组织一个sql
        String sql1 = "update account set balance = balance - 100 where id = 1";
        String sql2 = "update account set balance = balance + 100 where id = 2";
        PreparedStatement preparedStatement = null;
        //3. 创建一个PreparedStatement对象
        try {
            connection = JDBCUtils.getConnection();
            //将connection设置为不自动提交
            connection.setAutoCommit(false);//开启了事务
            preparedStatement = connection.prepareStatement(sql1);
            preparedStatement.executeUpdate();//执行第一条sql语句
            //int i = 1 / 0; //抛出异常
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();//执行第二条sql语句
            connection.commit();
        } catch (Exception e) {
            //这里我们可以进行回滚,即撤销执行的SQL
            //不设置savepoint的情况下,默认回滚到事务开始的状态.
            System.out.println("执行发生异常,撤销执行的sql");
            try {
                connection.rollback();
            } catch (SQLException ex) {
                throw new RuntimeException(ex);
            }
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(null, preparedStatement, connection);
        }
    }
}
```

# 批处理

- 基本介绍

1. 当需要成批插入或者更新记录时。可以采用Java的批量更新机制，这一机制允许多条语句一次性提交给数据库批量处理。通常情况下比单独提交处理更有效率。
2. JDBC的批量处理语句包括下面方法：
   addBatch()：添加需要批量处理的SQL语句或参数
   executeBatch()：执行批量处理语句：
   clearBatch()：清空批处理包的语句
3. JDBC连接MySQL时，如果要使用批处理功能，请再url中加参数? rewriteBatchedStatements=true
4. 批处理往往和PreparedStatement一起搭配使用，可以既减少编译次数，又减少运行次数，效率大大提高



- 应用实例

1. 演示向admin2表中添加5000条数据，看看使用批处理耗时多久
2. 注意：需要修改 配置文件 jdbc.properties
   `url=jdbc:mysql://localhost:3306/数据库?rewriteBatchedStatements=true`

```mysql
-- 创建测试表
CREATE TABLE admin2(
id int primary key auto_increment,
username varchar(32) not null,
`password` VARCHAR(32) NOT NULL);
```

```java
import com.lypedu.jdbc.utils.JDBCUtils;
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class Batch_ {
    //传统方法,添加5000条数据到admin2
    @Test
    public void noBatch() throws Exception {
        Connection connection = JDBCUtils.getConnection();
        String sql = "insert into admin2 values(null,?,?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        System.out.println("开始执行");
        long start = System.currentTimeMillis();//开始时间
        for (int i = 0; i < 5000; i++) {
            preparedStatement.setString(1, "jack" + i);
            preparedStatement.setString(2, "666");
            preparedStatement.executeUpdate();
        }
        long end = System.currentTimeMillis();
        System.out.println("传统方式耗时为 = " + (end - start)); //传统方式耗时为 = 768
        //关闭连接
        JDBCUtils.close(null, preparedStatement, connection);
    }

    @Test
    public void Batch() throws Exception {
        Connection connection = JDBCUtils.getConnection();
        String sql = "insert into admin2 values(null,?,?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        System.out.println("开始执行");
        long start = System.currentTimeMillis();//开始时间
        for (int i = 0; i < 5000; i++) {
            preparedStatement.setString(1, "jack" + i);
            preparedStatement.setString(2, "666");
            //将sql语句加入到批处理包中 -> 看源码
            /*
            //1.第一次就创建 ArrayList - elementData => Object[]
            //2. elementData => Object[] 就会存放我们预处理的sqL语句
            //3.当elementData满后，就按照1.5扩容
            //4. 当添加到指定的值后，就executeBatch
            //5. 批量处理会减少我们发送sqL语句的网络开销，而且减少编译次数，因此效率提高
            
            public void addBatch() throws SQLException {
                try {
                        synchronized(this.checkClosed().getConnectionMutex()) {
                            QueryBindings<?> queryBindings = ((PreparedQuery)this.query).getQueryBindings();
                            queryBindings.checkAllParametersSet();
                            this.query.addBatch(queryBindings.clone());
                    }
                } catch (CJException var6) {
                    throw SQLExceptionsMapping.translateException(var6, this.getExceptionInterceptor());
                }
            }
             */
            preparedStatement.addBatch();
            //当有1000条记录时,再批量执行
            if ((i + 1) % 1000 == 0){ //满1000条sql
                preparedStatement.executeBatch();
                //清空一把
                preparedStatement.clearBatch();
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("批处理方式耗时为 = " + (end - start)); //批处理方式耗时为 = 64
        //关闭连接
        JDBCUtils.close(null, preparedStatement, connection);
    }
}
```

# 数据库连接池技术

- 5k次连接数据库问题

com.hspedu.jdbc.datasource   ConQuestion.java

1. 编写程序完成连接MySQL5000次的操作
2. 看看有什么问题，耗时又是多久  =＞  数据库连接池

> 传统获取Connection问题分析
>
> 1. 传统的JDBC数据库连接使用 DriverManager 来获取，每次向数据库建立连接的时候都要将 Connection 加载到内存中，再验证IP地址，用户名和密码（0.05S~1s时间）。需要数据库连接的时候，就向数据库要求一个，频繁的进行数据库连接操作将占用很多的系统资源，容易造成服务器崩溃。
> 2. 每一次数据库连接，使用完后都得断开，如果程序出现异常而未能关闭，将导致数据库内存泄漏，最终将导致重启数据库。
> 3. 传统获取连接的方式，不能控制创建的连接数量，如连接过多，也可能导致内仔泄漏，MySQL崩溃。
> 4. 解决传统开发中的数据库连接问题，可以采用数据库连接池技术（connection pool）.

```mysql
import com.lypedu.jdbc.utils.JDBCUtils;
import org.junit.jupiter.api.Test;
import java.sql.Connection;

public class ConQuestion {
    @Test
    public void testCon() {
        //看看 连接-关闭 connection 会耗用多久
        long start = System.currentTimeMillis();
        for (int i = 0; i < 5000; i++) {
            //使用传统的jdbc方式,得到连接
            Connection connection = JDBCUtils.getConnection();
            //做一些工作,比如得到PreparedStatement,发送sql
            //......
            //关闭
            JDBCUtils.close(null, null, connection);
        }
        long end = System.currentTimeMillis();
        System.out.println("传统方法耗时为 = " + (end - start)); //耗时为 = 47064
    }
}
```

> 基本介绍
>
> 1. 预先在缓冲池中放入一定数量的连接，当需要建立数据库连接时，只需从“缓冲池”中取出一个，使用完毕之后再放回去。
> 2. 数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是重新建立一个。
> 3. 当应用程序向连接池请求的连接数超过最大连接数量时，这些请求将被加入到等待队列

![image-20240306下午23939579](/Users/yannlau/Library/Application Support/typora-user-images/image-20240306下午23939579.png)

> • 数据库连接池种类
> 1. JDBC 的数据库连接池使用javax.sqLDataSource 来表示，DataSource只是一个接口，该接口通常由第三方提供实现
> 2. C3P0 数据库连接池，速度相对较慢，稳定性不错 （hibernate, spring）
> 3. DBCP数据库连接池，速度相对c3p0较快，但不稳定
> 4. Proxool数据库连接池，有监控连接池状态的功能，稳定性较c3p0差一点
> 5. BoneCP 数据库连接池，速度快
> 6. Druid（德鲁伊）是阿里提供的数据库连接池，集DBCP、C3P0、Proxool优点于一身的数据库连接池

![image-20240306下午31733220](/Users/yannlau/Library/Application Support/typora-user-images/image-20240306下午31733220.png)

```java
import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.junit.jupiter.api.Test;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class C3P0_ {
    //方式1 相关参数,在程序中指定,user url password等
    @Test
    public void testC3P0_01() throws Exception {
        //1. 创建一个数据源对象
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
        //2. 通过配置文件获取相关连接的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("/Users/yannlau/IdeaProjects/jdbc_learning/src/mysql.properties"));
        //读取相关的属性值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String driver = properties.getProperty("driver");
        // 给数据源 comboPooledDataSource 设置相关参数
        //注意: 连接是由 comboPooledDataSource 来管理的
        comboPooledDataSource.setDriverClass(driver);
        comboPooledDataSource.setUser(user);
        comboPooledDataSource.setPassword(password);
        comboPooledDataSource.setJdbcUrl(url);

        //设置初始化连接数和最大连接数
        comboPooledDataSource.setInitialPoolSize(10);
        comboPooledDataSource.setMaxPoolSize(50);
        // 测试连接池的效率,测试对mysql5000次操作
        long l = System.currentTimeMillis();
        for (int i = 0; i < 5000; i++) {
            Connection connection = comboPooledDataSource.getConnection();
            //System.out.println("连接成功");
            connection.close();
        }
        long e = System.currentTimeMillis();
        System.out.println("C3P0连接池5000次用时为 = " + (e - l));//300ms
    }

    //方式2 使用配置文件模版来完成
    //1. 将C3P0提供的c3p0.config.xml拷贝到src目录下
    //2. 该文件指定了连接数据库和连接池的相关参数
    @Test
    public void testC3P0_02() throws SQLException {
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource("lyp_mysql");
        //测试5000次连接mysql
        long start = System.currentTimeMillis();
        System.out.println("开始执行...");
        for (int i = 0; i < 5000; i++) {
            Connection connection = comboPooledDataSource.getConnection();
            //System.out.println("连接OK");
            connection.close();
        }
        long end = System.currentTimeMillis();
        System.out.println("c3p0的第二种方式 耗时 = " + (end - start)); //309ms
    }
}
```

```xml
<!-- 放在src文件夹下 -->
<c3p0-config>
  <!--使用默认的配置读取数据库连接池对象 -->
  <named-config name="lyp_mysql">
    <!--  连接参数 -->
    <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db01?useServerPrepStmts=true</property>
    <property name="user">root</property>
    <property name="password">sjywza123.</property>

    <!-- 连接池参数 -->
    <!-- 每次增长的连接数 -->
    <property name="acquireIncrement">5</property>
    <!--初始化申请的连接数量-->
    <property name="initialPoolSize">10</property>
    <!-- 最小连接数 -->
    <property name="minPoolSize">5</property>
    <!--最大的连接数量-->
    <property name="maxPoolSize">50</property>

    <!--超时时间-->
    <property name="checkoutTimeout">3000</property>

    <!-- 可连接的最多的命令对象数 -->
    <property name="maxStatements">5</property>
    <!-- 每个连接对象可连接的最多的命令对象数 -->
    <property name="maxStatementsPerConnection">2</property>

  </named-config>
</c3p0-config>
```

# 德鲁伊连接池

```java
import com.alibaba.druid.pool.DruidDataSourceFactory;
import org.junit.jupiter.api.Test;
import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.util.Properties;

public class Druid_ {
    @Test
    public void testDruid() throws Exception {
        //1. 加入 Druid 的jar包
        //2. 加入 配置文件 druid.properties ,将该文件拷贝到项目的src目录
        //3. 创建Properties对象,读取配置文件
        Properties properties = new Properties();
        properties.load(new FileInputStream("src/druid.properties"));
        //4. 创建一个指定参数的数据库连接池,Druid连接池
        DataSource dataSource =
                DruidDataSourceFactory.createDataSource(properties);
        long start = System.currentTimeMillis();
        for (int i = 0; i < 5000; i++) {
            Connection connection = dataSource.getConnection();
            //System.out.println("连接成功");
            connection.close();
        }
        long end = System.currentTimeMillis();
        System.out.println("Druid的5000次连接耗时为 = " + (end - start));//206ms
        //在测试数据量更大时,Druid的优势会更加明显
    }
}
```

```properties
#key=value
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai&zeroDateTimeBehavior=CONVERT_TO_NULL&allowPublicKeyRetrieval=true
#url=jdbc:mysql://localhost:3306/db01
username=root
password=sjywza123.
#initial connection Size
initialSize=10
#min idle connection Size
minIdle=5
#max active connection size
maxActive=20
#max wait time (5000 mil seconds)
maxWait=3000
```

> ```java
>     public static void main(String[] args) {
>         //1.得到链接
>         Connection connection = null;
>         //2. 组织一个sql
>         String sql = "select * from actor";
>         PreparedStatement preparedStatement = null;
>         ResultSet set = null;
>         //3. 创建一个PreparedStatement对象
>         try {
>             connection = JDBCUtilsByDruid.getConnection();
>             preparedStatement = connection.prepareStatement(sql);
>             //执行得到结果集
>             set = preparedStatement.executeQuery();
>             //遍历该结果集
>             while(set.next()){
>                 int id = set.getInt("id");
>                 String name = set.getString("name");
>                 String sex = set.getString("sex");
>                 Date borndate = set.getDate("borndate");
>                 String phone = set.getString("phone");
>                 System.out.println(id+"\t"+name+"\t"+sex+"\t"+borndate+"\t"+phone);
>             }
>         } catch (SQLException e) {
>             throw new RuntimeException(e);
>         } finally {
>             //关闭资源
>             JDBCUtilsByDruid.close(set, preparedStatement, connection);
>         }
>     }
> ```
>
> ```java
> import com.alibaba.druid.pool.DruidDataSourceFactory;
> import javax.sql.DataSource;
> import java.io.FileInputStream;
> import java.io.IOException;
> import java.sql.Connection;
> import java.sql.ResultSet;
> import java.sql.SQLException;
> import java.sql.Statement;
> import java.util.Properties;
> public class JDBCUtilsByDruid {
>     private static DataSource ds;
>     //在静态代码块完成 ds初始化
>     static{
>         Properties properties = new Properties();
>         try {
>             properties.load(new FileInputStream("src/druid.properties"));
>             ds = DruidDataSourceFactory.createDataSource(properties);
>         } catch (IOException e) {
>             throw new RuntimeException(e);
>         } catch (Exception e) {
>             throw new RuntimeException(e);
>         }
>     }
>     //编写getConnection方法
>     public static Connection getConnection() throws SQLException {
>         return ds.getConnection();
>     }
>     //关闭连接,强调: 在数据库连接池技术中,close不是真的断掉连接
>     //而是把使用的Connection对象放回连接池
>   	//因为这里的Connection其实是DruidPooledConnection而不是原生的Connection
>     public static void close(ResultSet resultSet, Statement statement,Connection connection){
>         try {
>             if (resultSet != null) {
>                 resultSet.close();
>             }
>             if (statement != null) {
>                 statement.close();
>             }
>             if (connection != null) {
>                 connection.close();
>             }
>         } catch (SQLException e) {
>             //将编译异常转成运行异常抛出
>             throw new RuntimeException(e);
>         }
>     }
> }
> ```

# Apache-DBUtils

> 问题描述
>
> 1. 关闭connection 后，resultSet 结果集无法使用
> 2. resultSet 不利于数据的管理

![image-20240307上午91725161](/Users/yannlau/Library/Application Support/typora-user-images/image-20240307上午91725161.png)

> 模拟Apache

```java
import com.lypedu.jdbc.utilsbydruid.JDBCUtilsByDruid;
import org.junit.jupiter.api.Test;
import java.sql.*;
import java.util.ArrayList;

public class Druid_Apache {
    //使用老师的土方法来解决ResultSet =封装=> ArrayList
    @Test
    public ArrayList<Actor> apacheAnalogy() {
        //1.得到链接
        Connection connection = null;
        //2. 组织一个sql
        String sql = "select * from actor where id > ?";
        PreparedStatement preparedStatement = null;
        ResultSet set = null;
        ArrayList<Actor> list = new ArrayList<Actor>();//创建ArrayList对象,存放actor对象
        //3. 创建一个PreparedStatement对象
        try {
            connection = JDBCUtilsByDruid.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setInt(1, 1);
            //执行得到结果集
            set = preparedStatement.executeQuery();
            //遍历该结果集
            while (set.next()) {
                int id = set.getInt("id");
                String name = set.getString("name");
                String sex = set.getString("sex");
                Date borndate = set.getDate("borndate");
                String phone = set.getString("phone");
                //把得到的ResultSet的记录封装到Actor对象,放入list集合中
                list.add(new Actor(id, name, sex, borndate, phone));
            }
            System.out.println("list集合数据:" + list);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭资源
            JDBCUtilsByDruid.close(set, preparedStatement, connection);
        }
        return list;
    }
}
```

```java
import java.sql.Date;

public class Actor {
    private Integer id;
    private String name;
    private String sex;
    private Date borndate;
    private String phone;

    public Actor() { //一定要给一个无参构造器[反射需要]
    }

    public Actor(Integer id, String name, String sex, Date borndate, String phone) {
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.borndate = borndate;
        this.phone = phone;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBorndate() {
        return borndate;
    }

    public void setBorndate(Date borndate) {
        this.borndate = borndate;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "Actor{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", borndate=" + borndate +
                ", phone='" + phone + '\'' +
                '}';
    }
}
```

> • 基本介绍
>
> 1. commons-dbutils 是 Apache 组织提供的一个开源JDBC工具类库，它是对JDBC的封装，使用dbutils能极大简化idbc编码的工作量 [真的] DbUtils类
>
> 1. QueryRunner类：该类封装了SQL的执行，是线程安全的。可以实现增、删、改、查、批处理
> 2. 使用QueryRunner类实现查询
> 3. ResultSetHandler接口：该接口用于处理java.sql.ResultSet，将数据按要求转换为另一种形式.

> ArrayHandler： 把结果集中的第一行数据转成对象数组。
> ArrayListHandler：把结果集中的每一行数据都转成一个数组，再存放到List中。
> BeanHandler：将结果集中的第一行数据封装到一个对应的JavaBean实例中。
> BeanListHandler：将结果集中的每一行数据都封装到一个对应的JavaBean实例中，存放到List里。
> ColumnListHandler：将结果集中某一列的数据存放到List中。
> KeyedHandler（name）：将结果集中的每行数据都封装到Map里，再把这些map再存到一个map里，其key为指定的key。
> MapHandler：将结果集中的第一行数据封装到一个Map里，key是列名，value就是对应的值。
> MapListHandler：将结果集中的每一行数据都封装到一个Map里，然后再存放到List

> 应用实例
>
> 使用DBUtils+数据连接池(德鲁伊)方式,完成对表actor的crud

```java
// 以下代码会有 sql.date->localdate错误等待解决
// 需要将 Actor 类中的 Date 改为 LocalDateTime 类型
import com.lypedu.jdbc.utilsbydruid.JDBCUtilsByDruid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import org.junit.jupiter.api.Test;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public class DBUtils_Use {
    //使用apache-DBUtils 工具类 + druid 完成对表的crud
    @Test
    public void testQueryMany() throws SQLException { //返回结果是多行的情况
        //1. 得到 连接 ( druid )
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用DBUtils类和接口, 先引入DBUtils相关的jar, 加入到本Project
        //3. 创建QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4. 就可以执行相关的方法, 返回 ArrayList 结果集
        String sql = "select * from actor where id >= ?";
        //老韩解读
        // (1) query方法就是执行sqL 语句，得到resultset ---封装到 --> ArrayList 集合中
        // (2)返回集合
        // (3) connection：连接
        // (4) sql : 执行sql语句
        // (5) new BeanListHandler<>(Actor.class):在将resultset -> Actor 对象 -> 封装到ArrayList
        //      底层使用反射机制 去获取Actor 类的属性，然后进行封装
        // (6) 1 就是给；SqL 语句中的？赋值，可以有多个值，因为是可变参数
        // (7) 成层得到的resultset，会在query 关闭，关闭PreparedStatment
        List<Actor> list = queryRunner.query(connection, sql, new BeanListHandler<>(Actor.class), 1);
        for (Actor actor : list) {
            System.out.println(actor);
        }
        //关闭连接
        JDBCUtilsByDruid.close(null, null, connection);
    }

    // 演示 apache-DBUtils + druid 完成返回的结果是单行记录(单个对象)
    @Test
    public void testQuerySingle() throws SQLException {
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用DBUtils类和接口, 先引入DBUtils相关的jar, 加入到本Project
        //3. 创建QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4. 就可以执行相关的方法, 返回单个对象
        String sql = "select * from actor where id = ?";
        // 因为我们返回的单行记录<--->单个对象,使用的 Handler 是 BeanHandler
        Actor actor = queryRunner.query(connection, sql, new BeanHandler<>(Actor.class), 3);
        System.out.println(actor);
        //释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }

    // 演示 apache-DBUtils + druid 完成返回的结果是单行单列记录(单个对象)->返回的即使Object
    @Test
    public void testScalar() throws SQLException {
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用DBUtils类和接口, 先引入DBUtils相关的jar, 加入到本Project
        //3. 创建QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4. 就可以执行相关的方法, 返回单行单列,返回的就是Object
        String sql = "select name from actor where id = ?";
        // 因为我们返回的单行记录<--->单个对象,使用的 Handler 是 BeanHandler

        Object obj = queryRunner.query(connection, sql, new ScalarHandler<>(), 3);
        System.out.println(obj);
        //释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }

    //演示apache-dbutils + druid 完成 dml
    @Test
    public void testDML() throws SQLException {
        //1. 得到连接 (druid)
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用DBUtils类和接口, 先引入DBUtils相关的jar, 加入到本Project
        //3. 创建QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4.组织sql 完成 update, insert , delete
        String sql = "update actor set name = ? where id = ?";
        //(1) 执行dml操作是queryRunner.update()
        //(2) 返回的值是受影响的行数(affected 受影响)
        int affectedRow = queryRunner.update(connection, sql, "林青霞", 6);
        System.out.println(affectedRow > 0 ? "执行成功" : "执行没有影响到表");
        //释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }
}
```

# DAO

- 问题分析

apache-dbutils+Druid 简化了JDBC开发，但还有不足：

1. SQL语句是固定，不能通过参数传入，通用性不好，需要进行改进，更方便执行 增删改查
2. 对于select 操作，如果有返回值，返回类型不能固定，需要使用泛型
3. 将来的表很多，业务需求复杂，不可能只靠一个Java类完成
4. 引出=》BasicDAO 画出示意图, 看看实际开发中, 如何处理

- 基本说明

1. DAO:data access object数据访问对象

2. 这样的通用类，称为BasicDao，是专门和数据库交互的，即完成对数据库（表）的crud操作。
3. 在BaiscDao 的基础上，实现一张表 对应一个Dao，更好的完成功能，比如 Customer表-
   Customer.java (javabean)-CustomerDao.java

![image-20240307下午71119411](/Users/yannlau/Library/Application Support/typora-user-images/image-20240307下午71119411.png)

![image-20240307下午30335880](/Users/yannlau/Library/Application Support/typora-user-images/image-20240307下午30335880.png)

> 自己实现一个模拟的BasicDAO
>
> 完成一个简单设计 com.hspedu.dao
>
> 1. com.hspedu.dao_.utils // 工具类
>
> 2. com.hspedu.dao_.domain // javabean
> 3. com.hspedu.dao_.dao // 存放XxxDAO 和 BasicDAO
> 4. com.hspedu.dao_.test // 写测试类

```java
package com.lypedu.dao_.dao;

import com.lypedu.dao_.utils.JDBCUtilsByDruid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;
public class BasicDao<T> {  //泛型指定具体的类型
    private QueryRunner qr = new QueryRunner();
    //开发通用的dml方法,针对任意的表
    public int update(String sql, Object... parameters) {
        Connection connection = null;
        try {
            connection = JDBCUtilsByDruid.getConnection();
            int update = qr.update(connection, sql, parameters);
            return update;
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtilsByDruid.close(null, null, connection);
        }
    }
    // 返回多个对象(即查询的结果是多行),针对任意表
    /**
     * @param sql        sql语句 可以有?
     * @param clazz      传入一个Class对象,底层反射需要 Actor.class
     * @param parameters 传入 ? 的具体的值,可以是多个
     * @return 根据 Actor.class 返回对应的 ArrayList
     */
    public List<T> queryMulti(String sql, Class<T> clazz, Object... parameters) {
        Connection connection = null;
        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.query(connection, sql, new BeanListHandler<T>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtilsByDruid.close(null, null, connection);
        }
    }
    //查询单行结果的通用方法
    public T querySingle(String sql, Class<T> clazz, Object... parameters) {
        Connection connection = null;
        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.query(connection, sql, new BeanHandler<T>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtilsByDruid.close(null, null, connection);
        }
    }
    // 查询单行单列的方法,即返回单值的方法
    public Object queryScalar(String sql, Object... parameters){
        Connection connection = null;
        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.query(connection, sql, new ScalarHandler<>(), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtilsByDruid.close(null, null, connection);
        }
    }
}

package com.lypedu.dao_.dao;

import com.lypedu.dao_.domain.Actor;

public class ActorDAO extends BasicDao<Actor> {
    //1. 就有BasicDAO的方法
    //2. 根据业务需求,可以编写特有的方法
}


package com.lypedu.dao_.test;

import com.lypedu.dao_.dao.ActorDAO;
import com.lypedu.dao_.domain.Actor;
import org.junit.jupiter.api.Test;

import java.util.List;

public class TestDAO {
    //测试ActorDAO对actor表进行crud操作
    @Test
    public void testActorDAO() {
        ActorDAO actorDAO = new ActorDAO();
        //1. 查询
        List<Actor> actors = actorDAO.queryMulti("select * from actor where id >= ?", Actor.class, 1);
        for (Actor actor : actors) {
            System.out.println(actor);
        }
        //2. 查询单行记录
        Actor actor = actorDAO.querySingle("select * from actor where id = ?", Actor.class, 3);
        System.out.println(actor);
        //3. 查询单行单列
        Object object = actorDAO.queryScalar("select name from actor where id = ?", 2);
        System.out.println(object);
        //4. dml
        int update = actorDAO.update("insert into actor values(null,?,?,?,?)", "张无忌", "男", "2000-11-11", "999");
        System.out.println(update > 0 ? "执行成功" : "执行没有影响到数据库");
    }
}
```

