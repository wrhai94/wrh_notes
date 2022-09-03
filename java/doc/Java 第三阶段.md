# 网络编程
## 网络基础
- 网络通信
  - 两台设备通过网络实现数据传输
  - 网络通信：将数据通过网络从一台设备传输到另一台设备
  - java.net
- 网络
  - 多台设备通过一定物理设备连接起来构成物理
  - 根据物理的覆盖范围不同，对网络进行分类
    - 局域网
    - 城域网
    - 广域网：覆盖全国，设置全球，如万维网
- IP 地址
  - 用于唯一标识网络中的主机
  - IPV4 4个字节，32位，一个字节 0~255，点分十进制
  - IP 地址组成：网络地址 + 主机地址
  - IPV6 替代 IPV4，16 个字节，128位，冒分十六进制
  - ![IP 分类](F:/note/file/java/MyJava//img/IP%20分类.png)
- 域名
- 端口
  - 标识计算机上特定的程序
  - 范围 0 ~ 65535，2个字节
  - 开发中，最好不要使用0~1024端口，可能被一些程序占用了
- TCP 协议
  - 使用 TCP 协议前，须先建立 TCP 连接，形成传输数据通道
  - 传输前，采用三次握手方式，**可靠**
  - TCP 协议进行通信的两个应用进程：客户端，服务端
  - 在连接中可进行大数据传输
  - 传输完毕，需释放已建立的连接，**效率低**
- UDP 协议，用户数据协议
  - 将数据，源，目的封装成数据包，不需要建立连接，**不可靠**
  - 每个数据包大小限制在 64K 内，不适合传输大量数据
  - 发生数据结束后，无需释放连接，**速度快**

## InetAddress
- 获取本机的 InetAddress 对象
  InetAddress localhost = InetAddress.getLocalHost();
- 根据主机名/域名获取 InetAddress 对象
  InetAddress byName = InetAddress.getByName("DESKTOP-GD34TQC");
  InetAddress baidu = InetAddress.getByName("www.baidu.com");

## Socket
- 套接字，开发网络应用程序被广泛采用，以至于成为事实上的标准
- 通信的两端都要有 Socket,是两台机器间的断点
- 网络通信其实 socket 间的通信
- Socket 允许程序把网络连接当成一个流，数据在两个 Socket 间通过 IO 传输
- 一般主动发起通信的应用程序属客户端，等待通信请求的为服务端

## TCP 编程
### 字节流
#### 服务端
- 在服务端监听某个端口，等待连接
  ServerSocket serverSocket = new ServerSocket(9999);
- 当没有客户端连接该端口，程序会阻塞，等待连接。当有客户端连接，则会返回 Socket 对象，程序继续执行。可以通过 accept() 返回多个 Socket（多个客户端连接服务器的并发）。
  Socket socket = serverSocket.accept();
- 通过 Socket 对象读取客户端写入到数据通道的数据。
  InputStream inputStream = socket.getInputStream();
- IO 读取
- 获取 socket 相关联的输出流
  OutputStream outputStream = socket.getOutputStream();
  outputStream.write("hi".getBytes());
  socket.shutdownOutput();// 设置输出结束标记
- 关闭流和 socket,serverSocket

#### 客户端
- 连接服务端,如果连接成功，返回 Socket 对象
  Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
- 通过 Socket 对象得到和 Socket 对象关联的输出流对象
  OutputStream outputStream = socket.getOutputStream();
- 通过输出流写入数据到数据通道
  OutputStream.write("Hello!".getBytes());
  socket.shutdownOutput();  //注意设置输出结束标记，否则服务端不知道输入的信息什么时候结束，会阻塞
- 获取和socket 相关联的输入流
  InputStream inputStream = socket.getInputStream();
  byte[] buf = new byte[1024];
  int readLen = 0;
  whlie (readLen = inputStream.read(buf) != -1) {
    String read = new String(buf, 0, readLen); 
  }
- 关闭流对象和 Socket
  outputStream.close();
  socket.close();

### 字符流
#### 输出
- 将 OutputStream 通过 OutputStreamWriter 转成 Writer,再包装成 BufferedWriter.
- newLine() 插入换行符，表示写入的内容结束，注意，要求对方使用readLine() 读取 ！！
- 使用字符流需要调用 flush() 手动刷新，否则数据不会写入数据通道。
- 不需要 shutdownOutput

#### 输入
- 将 InputStream 通过 InputStreamReader 转成 Reader,再包装成 BufferedReader.
- readLine

### 文件上传
- 文件上传分析
  ![文件上传图解](F:/note/file/java/MyJava/img/文件上传图解.jpg)
- 客户端读取文件到内存，转成文件字节数组，再将内存的文件字节数组写入数据通道。注意，因为是字节流，需要shutdownOutput
  ![输入流字节数组](F:/note/file/java/MyJava/img/输入流字节数组.jpg)
- 服务端读取数据通道的文件，转成文件字节数组，在把内存中的文件写入磁盘。
  ![上传文件服务端](F:/note/file/java/MyJava/img/上传文件服务端.jpg)
### TCP 编程常用指令
- netstat
  - netstat -an 可以查看当前主机网络情况，包括端口监听情况和网络连接情况
  - netstat -an | more 可以分页显示，按空格显示下一页
  - 外部地址，显示连接主机某个地址的连接的IP地址（可以是本机）
  - 状态，LISTENING 监听；ESTABLISHED 已连接
  - netstat -anb 查看什么程序在监听

### 其他细节
- 当客户端连接到服务端后，实际上客户端也是通过一个端口和服务端进行通讯，这个端口由 TCP/IP 随机分配。

## UDP 编程（了解）

# 反射
## 反射机制*
![反射机制原理](F:/note/file/java/MyJava/img/反射机制原理.jpg)
- 反射机制允许程序在执行期借助与 Reflection API 取得任何类的内部信息，并能操作对象的属性及方法。反射在设计模式和框架底层都会用到
- 加载完类之后，在堆中就产生一个 Class 类型的对象（一个类只有一个 Class 对象），这个对象包含了类的完整的结构信息，通过这个对象得到类的结构。这个 Class 对象就像一面镜子，通过它看到类的结构，所以称之为反射。
- Java 反射机制可以完成
  - 在运行时判断任意一个对象所属的类
  - 在运行时构造任意一个类的对象
  - 在运行时得到任意一个类所具有的成员变量和方法
  - 在运行时调用任意一个对象的成员变量和方法
  - 生成动态代理
- 反射相关的主要类
  - java.lang.Class
  - java.lang.reflect.Method
  - java.lang.reflect.Field
  - java.lang.reflect.Constructor
- 优缺点
  - 可以动态的创建和使用对象，使用灵活，框架技术的底层支撑
  - 反射基本是解释执行，对执行速度有影响。
  - Method 和 Field 可以设置 Accessible,取消访问检查，加快运行速度，不过效果不明显


## Class 类*
- Class 类也是一个类，继承了 Object
- Class 类对象不是 new 出来的，而是系统创建的
- 对于某个类的 Class 对象，在内存中只会存在一份，类只会加载一次
- 每个类的实例都会记得自己是有哪个 Class 实例所生成的
- 通过 Class 对象可以完整的得到一个类的完整结构
- Class 是存放在堆中的
- 类的字节码二进制数据是存放在方法区的

### 常用方法
- Class.forName : 通过全类名获取对应的 Class 对象
- newInstance : 创建对象实例
- getField : 根据属性名拿到属性
- getFields : 拿到所有属性

### 获取 Class 对象的方式
- Class.forName
- 类名.class,如 String.class
- 对象名.getClass()
- 通过类加载器获取
- 基本数据类型.class
- 基本数据类型的包装类.TYPE


## 类加载*
![类加载](F:/note/file/java/MyJava/img/类加载.png)
### 静态加载和动态加载
- 静态加载：编译时加载相关的类，如果没有则报错，依赖性强，如在代码中显式的编写这个类。
- 动态加载：运行时加载需要的类，如果运行时不用到该类，不存在该类也不报错，依赖性弱，如用反射加载类。

### 加载阶段
- JVM 在该阶段的主要目的是将字节码从不同的数据源转化为二进制字节流，加载到内存中，并生成一个 Class 对象。

### 连接阶段-验证
- 确保 Class 文件的字节流中包含信息符合当前虚拟机的要求，并不会危害虚拟机自身的安全
- 文件格式验证，元数据验证，字节码验证和符合引用验证

### 连接阶段-准备
- JVM 会在该阶段对静态变量分配内存并初始化（0，null），静态常量会直接赋值显式的值，因为常量赋值后就不能改变了。

### 连接阶段-解析
- 虚拟机将变量池内的符号引用替换为直接引用的过程

### 初始化
- 这个阶段才真正开始执行类中定义的程序代码，此阶段是执行\<clinit\>() 方法的过程
- \<clinit\>() 方法是由编译器按语句在源文件中出现的顺序，依次自动收集类中的所有静态变量的赋值动作和静态代码块中的语句
- 虚拟机会保证一个类的<clinit>() 方法在多线程环境中被正确地加锁、同步。


## 反射获取类的结构信息*
### Class
![Class类](F:/note/file/java/MyJava/img/Class类.png)
### Field
![Field](F:/note/file/java/MyJava/img/Field.png)
### Method
![Method](F:/note/file/java/MyJava/img/Method.png)
### Constructor
![Constructor](F:/note/file/java/MyJava/img/Constructor.png)
![反射创建对象](F:/note/file/java/MyJava/img/反射创建对象.png)
![反射创建对象2](F:/note/file/java/MyJava/img/反射创建对象2.png)
### 访问属性
![访问属性](F:/note/file/java/MyJava/img/访问属性.png)
### 访问方法
![访问方法](F:/note/file/java/MyJava/img/访问方法.png)

# JDBC 和连接池
- JDBC 为了访问不同的数据库提供了统一的接口，为了使用者屏蔽了细节问题
- Java 程序员使用JDBC，可以理解任何提供了 JDBC 驱动程序的数据库系统，从而完成对数据库的各种操作
![JDBC原理图.jpg](F:/note/file/java/MyJava/img/JDBC原理图.jpg)
![JDBC概述.jpg](F:/note/file/java/MyJava/img/JDBC概述.jpg)
## JDBC API
- JDBC API 是一系列的接口，它统一和规范了应用程序与数据库的连接、执行 SQL 语句，并得到返回结果等各类操作，相关类和接口在 java.sql 与 javax.sql 包中。
- 编写步骤
  - 注册驱动 - 加载 Driver 类
  - 获取连接 - 得到 Connection 
  - 执行增删改查 - 发送 SQL 给 mysql 执行
  - 释放资源 - 关闭相关连接
- 编写 JDBC 前需要引入Mysql的驱动包，如 mysql-connector-java-8.0.19.jar
```java
// 注册驱动
// 方式1
// Driver driver = new Driver();
// 方式2，使用反射加载,动态加载，更加灵活
Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
Driver driver = (Driver) aClass.newInstance();

// 得到连接
/*
jdbc:mysql -- 规定好表示协议，通过jdbc方式连接mysql
localhost -- 主机，IP地址
3306 -- mysql 监听的端口
wrh_bd01 -- 指定数据库
mysql 连接本质是 socket 连接
*/
String url = "jdbc:mysql://localhost:3306/wrh_db01";
Properties properties = new Properties();
properties.setProperty("user", "root");
properties.setProperty("password", "kingdee");
//  直接使用 Driver
// Connection connect = driver.connect(url, properties);
// 方式3 使用 DriverManager
DriverManager.registerDriver(driver);
Connection connect = DriverManager.getConnection(url,properties);

// 执行数据库操作
String sql = "select * from t_person";
// Statement 用于执行静态 SQL 语句并返回结果对象
Statement statement = connect.createStatement();
ResultSet resultSet = statement.executeQuery(sql);

// 关闭连接
resultSet.close();
statement.close();
connect.close();
```

```java
// 方式4，推荐
// 注册驱动,使用反射加载
// 在加载类时，com.mysql.jdbc.Driver 在静态代码块中执行了DriverManager.registerDriver(driver);，所以不需要再注册。
// mysql 驱动 5.1.6 开始可以无需写Class.forName("com.mysql.jdbc.Driver");
// jdk1.5 以后使用了 jdbc4 ,会自动调用驱动 jar 包下 META-INF\service\java.sql.Driver 文本中的类名去注册
// 还是建议写上
Class.forName("com.mysql.jdbc.Driver");
String url = "jdbc:mysql://localhost:3306/wrh_db01";
Properties properties = new Properties();
properties.setProperty("user", "root");
properties.setProperty("password", "kingdee");

Connection connect = DriverManager.getConnection(url, properties);
```

```java
//方式5 使用 properties 配置
Properties properties = new Properties();
properties.load(new FileInputStream("src\\mysql.properties"));

// 注册驱动,使用反射加载
Class.forName(properties.getProperty("driver"));
// 得到连接
Connection connect = DriverManager
        .getConnection(properties.getProperty("url"), properties.getProperty("user"), properties.getProperty("password"));
```
```properties
user=root
password=kingdee
url=jdbc:mysql://localhost:3306/wrh_db01
driver=com.mysql.jdbc.Driver
```

### ResultSet 结果集
- ResultSet 对象保存一个光标指向当前的数据行，光标最初位于第一行之前。
- next() 方法将光标移动到下一行，当存在下一行时返回 true,否则返回 false。
- previous() 移动到上一行
- 数据存放在 rows(ArrayList).elementData(对象数组)。

### Statement 接口
- Statement 对象，用于执行静态 SQL。
- 在连接建立后，需要对数据库进行访问，执行 命令或是 SQL 语句通过
  - Statement -- 存在 SQL 注入的问题,开发中基本不用
  - PreparedStatement -- 预处理
  - CallableStatement -- 存储过程

### PreparedStatement 接口
- PreparedStatement 执行的 SQL 语句中，参数用 ? 来表示，调用PreparedStatement 对象的 setXxx(索引, 值) 来设置这些参数
- executeQuery(),返回结果集
- executeUpdate(),增删改，返回影响的行数
- excute(sql),执行任意 sql ,返回布尔值
- 好处
  - 减少字符串拼接
  - 避免 sql 注入的问题
  - 减少编译
### JDBCUtils
- 可以写一个 JDBC 工具类，提供获取数据库连接和关闭连接的方法。
```java
public class JDBCUtils {
    private static String user;
    private static String password;
    private static String url;
    private static String driver;

    static {
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("src\\mysql.properties"));
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            url = properties.getProperty("url");
            driver = properties.getProperty("driver");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static Connection getConnection() throws ClassNotFoundException, SQLException {
        Class.forName(driver);
        return DriverManager.getConnection(url, user, password);
    }

    public static void close(ResultSet rs, PreparedStatement ps, Connection con) throws SQLException {
        if(rs != null) {
            rs.close();
        }
        if(ps != null) {
            ps.close();
        }
        if(con != null) {
            con.close();
        }
    }
}
```

## 事务*
- JDBC 程序中当一个 Connection 对象创建时，默认情况下是自动提交事务，每次执行一个 SQL 语句，如果执行成功，就会向数据库自动提交。
- JDBC 程序中为了让多个 sql 语句作为一个整体执行，需要使用事务
- 调用 Connection 的 setAutoCommit(false) 可以取消自动提交事务
- 所有 SQL 都执行成功后，调用 commit() 方法提交事务
- 在其中某个操作失败或出现异常，调用 rollback([保存点]) 方法回滚事务，可以设置回滚到保存点，默认回滚到事务开始状态。


## 批处理
- 该机制允许多条语句一次性提交给数据库批量处理,减少发送 sql 语句的网络开销。通常情况下比单独提交处理更有效率
- 批处理语句
  - addBatch -- 添加需要批量处理的 sql 语句或参数
  - executeBatch -- 执行批量处理语句
  - clearBatch -- 清空批量处理语句
- JDBC 连接 MYSQL 时，如果要使用批处理功能，需要在 url 中添加参数?rewriteBatchedStatements = true

```java
Connection connection = JDBCUtils.getConnection();
String sql = "insert into tb_student values (null, ?, ?)";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
long start = System.currentTimeMillis();
for (int i = 0; i < 1000; i++) {
    preparedStatement.setString(1, String.valueOf(i));
    preparedStatement.setString(2, "学生" + i);
    // preparedStatement.execute(); // 单独提交方式
    preparedStatement.addBatch();
    // 每 1000 行提交
    if((i + 1) % 1000 == 0){
        preparedStatement.executeBatch();
        // 执行完清理
        preparedStatement.clearBatch();
    }
}
long end = System.currentTimeMillis();
System.out.println("耗时 = " + (end - start));
JDBCUtils.close(null, preparedStatement, connection);
```
```properties
user=root
password=kingdee
# 批处理一定要加上参数?rewriteBatchedStatements=true，否则批处理不生效
url=jdbc:mysql://localhost:3306/wrh_db01?rewriteBatchedStatements=true
driver=com.mysql.jdbc.Driver
```

## 连接池*
- 传统获取连接的问题
  - 使用 DriverManager 来获取连接，每次向数据库建立连接的时候都要将 Connection 加载到内存中，再验证 IP地址，用户名和密码。需要数据库连接的时候，就向数据库要求一个，频繁进行数据库连接操作将占用很多的系统资源，容易造成服务器崩坏。
  - 每次数据库连接使用完后都得断开，如果程序异常未能关闭，将导致数据库内存泄露，最终导致重启数据库。
  - 传统方式不能控制创建连接的数量，如果连接数过多，可能导致内存泄露。

- 连接池概念
  - 预先在缓冲池中放入一定数量的连接，当需要建立数据量连接，只需要从缓冲池获取一个。
  - 数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是重新建立一个。
  - 当应用程序向连接池请求的连接数超过最大连接数量时，这些请求会被加入到等待队列中。

- 连接池种类
  - JDBC 的数据库连接池使用 javax.sql.DataSource 来表示，DataSource 只是一个接口，通常又第三方提供实现（所以需要提供下列连接池类型相应的 jar 包）。
  - C3P0，速度相对较慢，稳定性好（hibernate，spring）
  - DBCP,较 C3P0 快，不稳定
  - Proxool,有监控连接池的功能，稳定性较 C3P0 差
  - BoneCP ,速度快
  - Druid,阿里提供的数据库连接池，集DBCP,C3P0,Proxool的优点于一身。

### C3P0
- 加入 C3P0 的 jar 包
```java
Properties properties = new Properties();
properties.load(new FileInputStream("src\\mysql.properties"));
String user = properties.getProperty("user");
String password = properties.getProperty("password");
String url = properties.getProperty("url");
String driver = properties.getProperty("driver");

ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
comboPooledDataSource.setDriverClass(driver);
comboPooledDataSource.setJdbcUrl(url);
comboPooledDataSource.setUser(user);
comboPooledDataSource.setPassword(password);

// 初始化连接数
comboPooledDataSource.setInitialPoolSize(10);
// 最大连接数
comboPooledDataSource.setMaxPoolSize(50);
Connection connection = comboPooledDataSource.getConnection();
// 数据库操作
// 关闭连接
connection.close();
```
```java
// 方式2 通过配置文件的方式,推荐使用
// 1. 将c3p0提供的 c3p0-config.xml 复制到 src 目录下
// 2. 该文件指定了连接数据库和连接池的相参数
ComboPooledDataSource wrh_db01 = new ComboPooledDataSource("wrh_db01");
Connection connection = wrh_db01.getConnection();
connection.close();
```

## 德鲁伊 Druid
- 开发中推荐使用，速度较快
- druid.properties 可以放在 main 下的 resources,如果没有该目录，右键工程，New - Source folder, folder name 输入 src/main/resources，完成创建。读入配置文件时，直接使用文件名读取。
```java
//1. 引入 Druid jar 包
//2. 加入配置文件,在官网找模板,xxx.properties
//3. 读取配置文件
Properties properties = new Properties();
properties.load(new FileInputStream("src\\druid.properties"));
DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
Connection connection = dataSource.getConnection();
connection.close(); //不是真的断掉连接，而是将连接放回连接池中
```
```java
// 德鲁伊工具类
import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Properties;

/**
 * @Author Wang
 * @Date 2022/7/24
 */
public class JDBCUitlByDuid {
    private static DataSource ds = null;

    static {
        Properties properties = new Properties();
        try {
            // 有时候 new FileInputStream("src\\druid.properties") 无法读取到文件，可以使用这个方式
            //  InputStream is = PropertiesUtil.class.getClassLoader().getResourceAsStream("druid.properties");
            properties.load(new FileInputStream("src\\druid.properties"));
            ds = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    public static void close(ResultSet rs, PreparedStatement ps, Connection con) {
        try {
            if(rs != null) {
                rs.close();
            }
            if(ps != null) {
                ps.close();
            }
            if(con != null) {
                con.close();
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}

```

## Apache-DBUtils
- 解决的问题
  - 结果集和连接是关联的，连接关闭了，结果集就无法使用
  - 结果集只能使用一次，不利于管理
  - 结果集使用不方便
- 介绍
  - commons-dbutils 是 Apache 组织提供的一个开源 JDBC 工具类库，它是对 JDBC 的封装，极大简化 jdbc 编码工作量
  - QueryRunner 类封装了SQL 的执行，线程安全，可实现增删查改，批处理
  - ResultSetHandler 接口用于处理 java.sql.ResultSet,将数据按要求转换为另一种形式。


### 查询
![DBUtils_1.jpg](F:/note/file/java/MyJava/img/DBUtils_1.jpg)
![DBUtils_2.jpg](F:/note/file/java/MyJava/img/DBUtils_2.jpg)
![DBUtils_Single.jpg](F:/note/file/java/MyJava/img/DBUtils_Single.jpg)
- 单行单列 ScalarHandler()

### DML
- queryRunner.update(connection, sql, params..); // 返回影响的行数，可以执行 dml 语句

## DAO 增删改查-BasicDao
- 解决的问题
  - SQL 语句时固定，不能通过参数传入，通用性差
  - 对于 select 操作，如果有返回值，返回类型不能固定，需要使用泛型
  - 将来表很多，业务需求复杂，不可能只靠一个Java 类完成
- 说明
  - DAO : data access object 数据访问对象
  - 通用类 BasicDao,专门和数据库交互，完成对数据库表的 crud 操作
  - 在 BasicDao 的基础上，实现一张表对应一个 Dao
![DAO示意图.jpg](F:/note/file/java/MyJava/img/DAO示意图.jpg)

# 正则表达式
- 用某种模式去匹配字符串的一个公式
## 基本语法
```java
String content = "tile=\"1998年12月到2000年2月\"";
// 创建正则表达式模式对象
Pattern pattern = Pattern.compile("title=\"(\\S*)\"");
// 创建匹配器
Matcher matcher = pattern.matcher(content);
while (matcher.find()){
    System.out.println(matcher.group(1));
}
```
- matcher.find()
  - 根据指定规则，定位满足规则的字符串。
  - 找到后将子字符串的开始索引记录到 matcher 对象的属性 int[] groups;
  groups[0]记录该字符串的开始索引，groups[1] 记录该字符串的结束索引+1.
  - 当规则有分组时，比如(19)(98)
    - groups[0]=0,groups[1] 记录结束索引+1，如4。
    - 记录第一组括号()匹配到的字符串groups[2]=0,groups[3]=2
    - 记录第二组括号()匹配到的字符串groups[4]=2,groups[5]=4，以此类推。
  - 同时记录 oldLast 的值为子字符串结束索引+1，下次执行 find 从该位置开始。

```java
public String group(int group) {
    if (first < 0)
        throw new IllegalStateException("No match found");
    if (group < 0 || group > groupCount())
        throw new IndexOutOfBoundsException("No group " + group);
    if ((groups[group*2] == -1) || (groups[group*2+1] == -1))
        return null;
    return getSubSequence(groups[group * 2], groups[group * 2 + 1]).toString();
}
```
- group
  - 根据 groups[0] 和 groups[1] 的指定的位置，从 content 开始截取子字符串，返回 [groups[0], groups[1])
  - 如果是分组
    - matcher.group(0),返回整段匹配到的字符串
    - matcher.group(1)，返回第一组匹配到的字符串
    - matcher.group(2)，返回第二组匹配到的字符串，以此类推
    - 取分组不能越界

### 转义号 \\\
- 转义符号，在我们使用正则表达式去检索某些特殊字符的时候，需要用到转义符号，否则检索不到结果
- java 正则表达式中，用两个斜杆表示，其它语言可能用一个斜杆。
- 需要用到转义符的字符：.*+()$?/\\[]^{}

### 元字符
#### 元字符-字符匹配符
![字符匹配1.png](F:/note/file/java/MyJava/img/字符匹配1.png)
![字符匹配2.png](F:/note/file/java/MyJava/img/字符匹配2.png)
![字符匹配3.png](F:/note/file/java/MyJava/img/字符匹配3.png)

#### 元字符-选择匹配符
![选择匹配符.jpg](F:/note/file/java/MyJava/img/选择匹配符.jpg)

#### 元字符-限定符
![限定符.jpg](F:/note/file/java/MyJava/img/限定符.jpg)
![限定符2.jpg](F:/note/file/java/MyJava/img/限定符2.jpg)
> {n,m} 会尽可能匹配到多的组合
> ? 紧跟在任意其它限定符之后时，匹配模式是“非贪心匹配”。
> 非贪心匹配，搜索到尽可能短的字符串
> 贪心匹配，搜索到尽可能长的字符串

#### 元字符-定位符
![定位符.jpg](F:/note/file/java/MyJava/img/定位符.jpg)

### 分组
### 捕获分组
![分组.jpg](F:/note/file/java/MyJava/img/分组.jpg)

### 非捕获分组
![非捕获分组.jpg](F:/note/file/java/MyJava/img/非捕获分组.jpg)

## 常用类*
### Pttern
- compile(regStr)
- matches(regStr, content),返回是否整体匹配成功。可以用于验证字符串是否满足格式
### Matcher
- start/end,匹配到字符串的起始位置和结束位置
- matches,是否满足整体匹配
- replaceAll(str),返回匹配到的字符串被替换为str的新字符串。
### PatternSyntaxException

## 分组、捕获、反向引用*
- 分组
  可以用圆括号组成一个匹配模式
- 捕获
  把正则表达式中分组匹配的内容，保存到内存中以数字编号或显式命名的组里，从左到右，以分组的左括号为标志，第一个出现的分组的组号为1，以此类推，0表示整个正则式。
- 反向引用
  圆括号的内容被捕获后，可以在这个括号后被使用，从而写出一个比较实用的匹配模式，这个我们称为反向引用。这种引用可以是正则表达式内部，也可以是在正则表达式外部，内部反向引用\\分组号，外部反向引用 $ 分组号。
  \\\分组号，如匹配连续5个相同的数字，(\\\d)\\\1{4},\\\d 匹配一个数字，\\\1 反向引用(\\\d),{4} 反向引用4次
  如匹配第一位和第四位相同，第二位和第三位相同，(\\\d)(\\\d)\\\2\\\1。
![反向引用应用1.jpg](F:/note/file/java/MyJava/img/反向引用应用1.jpg)

## String 的正则表达式，替换分割匹配
- string.replaceAll(regStr, str)，匹配到的子字符串用 str 替换
- string.matches(regStr) 验证字符串是否满足正则表达式，该方法是整体匹配，不加定位符也可以，但建议加上。
- string.split(regStr) 按正则表达式匹配到的字符串分割

```java
 /**
* 字符数 - +
* 不能以0开头，除了小于1的小数，如 0.1
*/
String str = "0.9342";
String reg = "^[-+]?([1-9]\\d*|0)\\.\\d+$";
if (str.matches(reg)){
  System.out.println("满足");
} else {
  System.out.println("不满足");
}
```
```java
String str = "http://www.baidu.com:8080/asd/index.html";
String reg = "^([a-zA-Z]+)://([a-zA-Z.]+):(\\d+)[\\w-/]*/([\\w.]+)$";
Pattern compile = Pattern.compile(reg);
Matcher matcher = compile.matcher(str);
if (matcher.matches()){
    System.out.println("满足格式:" + matcher.group(0));
    System.out.println("协议：" + matcher.group(1));
    System.out.println("域名：" + matcher.group(2));
    System.out.println("端口：" + matcher.group(3));
    System.out.println("文件：" + matcher.group(4));
} else {
    System.out.println("不满足格式");
}
```

