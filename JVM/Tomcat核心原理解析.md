 



## 1 Tomcat 基础

### 1.1  常见的 web 服务器

```
服务器软件 : 接收用户请求，处理请求，做出响应
常见的 web 服务器软件 ：
    webLogic、webSphere、JBOSS ： 大型的 JavaEE 服务器，支持所有的 JavaEE 规范，收费
    Tomcat ：中小型 JavaEE 服务器，支持少量 JavaEE 规范（servlet/jsp），开源免费
```

```
bin : 可执行文件，bat 批处理脚本， sh shell 脚本
webapps ： 默认的 web 应用部署目录
work ：存放 jsp 文件编译之后生成的 Java 源码以及 class 字节码文件的目录
```

![tomcat源码启动控制台中文乱码问题](C:\Users\Yolo\AppData\Roaming\Typora\typora-user-images\image-20200712181301788.png)







## 2 Tomcat 整体架构

### 2.1  Http 工作原理

HTTP 协议是浏览器与服务器之间的数据传输协议，主要规定了客户端和服务器之间的通信格式

```
1. 用户在浏览器输入请求地址
2. 浏览器发起 TCP 连接请求
3. 服务器接收请求并建立连接
4. 浏览器生成 http 格式的数据包并发送给服务器
5. 服务器解析数据包，执行请求，生成响应数据包，发送给浏览器
6. 浏览器解析 http 格式的数据包，将数据渲染之后响应给用户
```

### 2.2 Tomcat 整体架构

HTTP 服务器不直接调用业务类，而是把请求交给容器来处理，容器通过 Servlet 接口调用业务类，达到 HTTP 服务器与业务类解耦的目的。

Servlet 接口和 Servlet 容器这一整套规范叫做 Servlet 规范。Tomcat 按照 Servlet 规范实现了 Servlet 容器，同时它具有 HTTP 服务器的功能，程序员只需要实现一个 Servlet 并把它注册到 Tomcat（Servlet 容器）中， 剩下的事情就由 Tomcat 帮我们处理了。

```
Servlet 容器工作流程
------------------
当客户请求某个资源时，HTTP 服务器会用一个 ServletRequest 对象把客户的请求信息封装起来，Servlet 容器拿到请求后，根据请求的 URL 和 servlet 的映射关系，找到相应的 servlet，如果 servlet 还没有被加载，就利用反射机制创建这个 servlet，并调用 init 方法完成初始化，接着调用 service 方法处理请求，把结果封装成 ServletResponse 对象返回给 Http 服务器，服务器再响应给客户端。

```

Tomcat 设计了两个核心组件 —— Connector（连接器，负责对外交流） 和 Container（容器，负责内部处理）

### 2.3 连接器 —— Coyote 

Coyote 是 Tomcat 连接器框架的名称，是 Tomcat 提供的供客户端访问的外部接口，客户端通过 Coyote 与服务端建立连接、发送请求并接收响应。

Coyote 作为独立的模块，只负责具体协议和 IO 的相关操作。

EndPoint ：Coyote 通信端点，通信监听的接口，是具体 Socket 接收和发送的处理器，是对传输层的抽象，用来实现 TCP/IP 协议。

Processor ：Coyote 协议处理接口，用来实现 Http 协议，接收来自 EndPoint 的 Socket，读取字节流解析成 Tomcat Request 和 Response 对象，并通过 Adapter 将其交给容器处理。

### 2.4 容器 —— Catalina

Tomcat 是一系列可配置的组件构成的 Web 容器，Catalina 是 Tomcat 的 servlet 容器，其他模块都是为 Catalina 提供支撑的。

![Catalina 架构](D:\Program Files\Typora\notes\img\image-20200713202203718.png)

![image-20200713202354253](D:\Program Files\Typora\notes\img\image-20200713202354253.png)

###  2.5 Tomcat 启动流程

### 2.6 Tomcat 请求处理流程

