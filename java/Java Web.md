# Tomcat
- 需要先安装，官网下载解压即可。
- 目录结构
  - conf：配置文件
  - webapps：项目文件，一个文件夹一个项目
  - work：工作时目录，用来存放 Tomcat 运行时 jsp 翻译为 Servlet 的源码。
- 启动 Tomcat
  - 启动 Tomcat bin目录下的 startup.bat 文件，或者在DOS窗口bin 目录下自习 catalina run。
  - 打开浏览器输入以下地址验证是否启动成功
    - http://localhost:8080
    - http://127.0.0.1:8080
    - http://真实IP:8080
- 启动失败解决方法
  - startup.bat 启动后，窗口闪退。
    可能是 JAVA_HOME 配置错误。
- 修改端口
  找到配置文件 server.xml，修改 Connector 的 port
  ```xml
  <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
  ```
- 部署
  - 方式1：直接将过程拷贝到 \webapps 目录下，通过 Tomcat地址/过程目录 访问
  - 方式2：在 \conf\Catalina\localhost 下创建 xml 配置文件，指定访问路径和对应的工程目录
  ```xml
  <Context path="/访问路径" docBase="工程目录">
  ```
## 在 Idea 社区版搭建 javaweb 工程
[在 Idea 社区版搭建 javaweb 工程,并配置 Tomcat](./src/IDEA%20Community(%E7%A4%BE%E5%8C%BA%E7%89%88)%2Bmaven%E5%88%9B%E5%BB%BAJava%20web%E9%A1%B9%E7%9B%AE%E5%B9%B6%E9%85%8D%E7%BD%AETomcat%E5%85%A8%E8%BF%87%E7%A8%8B%20-%20Luquan%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD.html)
[在 Idea 社区版搭建 javaweb 工程,并配置 Tomcat](https://www.cnblogs.com/Luquan/p/12273595.html)


- 在 WEB-INF 下创建 lib 文件，用于存放第三方 jar 包。
- WEB-INF 目录是一个受服务器保护的目录，浏览器无法直接访问此目录内容。
- web.xml 是整个动态 web 工程的配置不是描述文件。

- 注意：
  使用 tomcat 10 在 pom.xml 中的 dependencies 元素中添加如下 dependency 元素，导入 servlet 5.0 依赖的包，servlet 版本过低会导致异常。或者下载低版本 tomcat。
  ```xml
  <dependency>
      <groupId>jakarta.servlet</groupId>
      <artifactId>jakarta.servlet-api</artifactId>
      <version>5.0.0</version>
    </dependency>
  ```

# Servlet
## 概述
- JavaEE 规范之一，即接口。
- JavaWeb 三大组件之一，Servlet 程序、Filter 过滤器和 Listener 监听器。
- Servlet 是运行在服务器上的一个 Java 程序，可以接受客户端发送过来的请求，并响应数据给客户端。

## 入门实例
- 创建一个类，实现 Servlet 接口。
- 在 web.xml 配置 Servlet 类和映射 url
  ```xml
  <servlet>
    <servlet-name>HelloServlet</servlet-name>
    <servlet-class>com.wrh.servlet.HelloServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>HelloServlet</servlet-name>
    <url-pattern>/helloServlet</url-pattern>
  </servlet-mapping>
  ```
- 访问
  tomcat 路径/工程路径(不填默认 ROOT 工程)/资源路径（不填默认 index.jsp/index.html）

## Servlet 生命周期
- 第一个到达服务器的 HTTP 请求被委派到 Servlet 容器。
- Servlet 容器在调用 service() 方法之前加载 Servlet。
- 然后 Servlet 容器处理由多个线程产生的多个请求，每个线程执行一个单一的 Servlet 实例的 service() 方法。
![Servlet-LifeCycle.jpg](F://note/file/java/javaweb/img/Servlet-LifeCycle.jpg)

> servlet 中可以通过 HttpServletRequest 的 getMethod 方法 获得请求的方式。
> HttpServlet 类的 service 方法通过 getMethod 将不同方式的请求分配给 doPost 或 doGet。

## ServletConfig 类
### 作用
- 可以获取 Servlet 程序的别名 servlet-name 的值
- 获取初始化参数 init-param（在 web.xml servlet 节点中配置）
- 获取 ServletContext 对象

> 可以通过 getServletConfig() 获取 ServletConfig 对象。
> 注意，重写 init 方法时，一定要调用父类方法，HttpServlet 的 init 方法会初始化 ServletConfig 对象。

## ServletContext 类
### 概述
- ServletContext 是一个接口，表示 Servlet 的上下文。
- 一个 web 工程，只有一个 ServletContext 对象实例
- ServletContext 对象是一个域对象（可以像 Map 一样存取数据的对象）

### 作用
- 获取 web.xl 中配置的上下文参数 context-param
- 获取当前工程路径
- 获取工程部署后在服务器磁盘的绝对路径
- 像 Map 一样存取数据。

## http 协议
### GET 请求
![Get请求.png](F://note/file/java/javaweb/img/Get请求.png)

### POST 请求
![POST请求.png](F://note/file/java/javaweb/img/POST请求.png)

### 响应
![响应.png](F://note/file/java/javaweb/img/响应.png)

## HttpServletRequest
- 每次请求进来， Tomcat 服务器都会创建一个 Request 对象传递给 Servlet 程序使用
- 解决中文乱码,获取参数前设置字符集，且必须在获取第一个参数前设置才有效， req.setCharacterEncoding("UTF-8");

### 请求转发
- 浏览器地址没有变化
- 他们是一次请求
- 他们共享 request 域中的数据
- 可以转发到 WEB-INF 目录下
- 无法访问工程外的资源
```java
req.setAttribute("key", "value");   // 设置域参数，可给转发目标 servlet 服务传参
RequestDispatcher requestDispatcher = req.getRequestDispatcher("/路径");
requestDispatcher.forward(req, resp);
```

#### 解决超链接使用转发跳转后，无法使用相对路径返回跳转回去
- 可以使用 base 设置页面相对路径参考地址,一般只需要到工程路径
```html
<head>
  <base href="http://localhost:8080/servlet01/[a.html]">
</head>
<body>
  <a href="../../index.html">返回首页</a>
</body>
```

> web 中 / 斜杆的意义
> 如果没浏览器解析，得到地址：http://ip:port/
> 
> 如果被服务器解析，得到地址：http://ip:port/工程路径
>   \<url-pattern>/servlet\</url-pattern>
>   servletContext.getRealPath("/");
> 
> 特殊情况：response.sendRediect("/"); 把斜杆发送给浏览器解析，得到 http://ip:port/。

## HttpServletResponse
- 每次请求进来， Tomcat 服务器都会创建一个 Response 对象传递给 Servlet 程序使用。
- 输出流
  - 字节流 getOutputStream() 常用于下载。
  - 字符流 getWriter() 常用于回传字符串。
  - 每次只能使用其中一个。
- 设置字符集
  - 服务器字符集 resp.setCharacterEncoding("UTF-8");
  - 通过请求头设置浏览器字符集 resp.setHeader("Content-Type", "text/html; charset=UTF-8");
  - 同时设置服务器和客户端字符集，还设置了响应头*  resp.setContentType("text/html; charset=UTF-8");
    此方法要在获取流之前使用才有效。

### 请求重定向
- 告诉浏览器访问另一个地址
```java
// 方法一
// 设置响应状态码 302， 表示重定向
resp.setStatus(302);
// 设置响应头，声明新地址在哪里
resp.setHeader("Location", "url");

// 方法二
resp.sendRedirect("url");
```
- 浏览器地址发生变化
- 发送2次请求
- 不共享 request 域中的数据
- 不能访问 WEB-INF
- 可以访问工程外的资源。

## JavaWeb 三层结构
![JavaWeb三层结构.png](F://note/file/java/javaweb/img/JavaWeb三层结构.png)

# JSP
## JSP 概述
- JSP 是 java server pages, java 服务器页面。
- 主要作用是替代 Servlet 程序回传 html 页面数据。
- jsp 本质
  - JSP 页面本质是一个 Servlet 程序
  - 当第一次访问一格 JSP 文件时， 在 CATALINA_BASE\work 目录下，可以找到该 JSP 页面被翻译成 java 文件和其 class 文件，它的类继承了 HttpJspBase 类，HttpJspBase 类又继承了 HttpServlet 类。