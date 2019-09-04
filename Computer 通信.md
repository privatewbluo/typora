## 书籍

1. 图解HTTP；
2. 图解TCP_IP;

---------------------------------

##    <strong style='color:lightblue;text-align:center;'>                                                                             图解HTTP</strong>

---------------------------------



#####Chapter one :了解web及网络基础

![](D:\Users\wbluo\Desktop\web html+css\web_1.png)

![但是](D:\Users\wbluo\Desktop\web html+css\web_2.png)

summary :通信之间[客户端+服务端]有许多协议：http +tcp/ip+dns

1. http ：主要解决了文本传输难题；

2. tcp【tranmission control protocol】/ip: 分层： 应用层、传输层、网络层和数据链路层；    ![](D:\Users\wbluo\Desktop\web html+css\web_9.png)     

3. ![](D:\Users\wbluo\Desktop\web html+css\web_10.png)   第一次握手：get 请求；第二次握手+第三次握手：put 响应

    <p style='text-align:center;color:orange;'>  建立连接：三次握手</p>

   ![](D:\Users\wbluo\Desktop\web html+css\shakehand.png)

   synchronize：同步

   第一个ack:告知客户端你可以从这个端口访问数据；

   第二个ack:客户端告知服务器OK，建立连接；

4. ![](D:\Users\wbluo\Desktop\web html+css\web_11.png)

5. ![](D:\Users\wbluo\Desktop\web html+css\web_12.png)





##### Chapter two :简单的http协议;

- 为什么会引入cookie?

  ​             ![sd](D:\Users\wbluo\Desktop\web html+css\web_3.png)

- ![](D:\Users\wbluo\Desktop\web html+css\web_4.png)

- ![](D:\Users\wbluo\Desktop\web html+css\web_5.png)

- 服务端会发送给客户端cookie, sid<b>  ---类似id 一样 Security Identifiers ;![](D:\Users\wbluo\Desktop\web html+css\web_6.png)

  服务器端像客户端发送Cookie是通过HTTP响应报文实现的，在Set-Cookie中设置需要像客户端发送的cookie，cookie格式如下：

  <p style='text-align:center;color:orange'>Set-Cookie: "<strong>name</strong>=value;<strong>domain</strong>=.domain.com;path=/;<strong>expires</strong>=Sat, 11 Jun 2016 11:29:42 GMT;HttpOnly;secure"</p>

  1. name:唯一sid；

  2. domain:访问那个域名；

  3. path:cookie存放路径；

  4. expire: cookies 失效时间：如果不设置这个时间戳，浏览就会在页面关闭时候删除所有的cookie; 我称之为session cookies ，会话cookies 不会存放到硬盘中，而是存放到内存当中；

     对于存放到内存当中的cookies ,可以被不同的浏览器调度使用----所以仅仅需要清除一次缓存就行；

     超过有效期则会无效，重新登录，再保存新的cookie;

  <p style='color:lightblue;'> 保存了cookies, 可以无需填入账户密码，可以保持通讯</p>

- 持久连接节省通信量

  1.    ![](D:\Users\wbluo\Desktop\web html+css\tcp_1.png)
  2.    ![](D:\Users\wbluo\Desktop\web html+css\tcp_2.png)
  3. ​    ![](D:\Users\wbluo\Desktop\web html+css\tcp_3.png)


##### Chapter three: HTTP 报文内的http 信息

![](D:\Users\wbluo\Desktop\web html+css\web_7.png)

​                                 ![](D:\Users\wbluo\Desktop\web html+css\web_8.png)

​              报文=首部(head)+主体（body)

- 请求报文（request) :who[user-agent]  asks for what [host ] with conditions[language,encoding type and so on ] 

  响应报文（response): when i send message for the request and coding type with status<br>![](D:\Users\wbluo\Desktop\web html+css\post.png)

- http协议的请求和响应报文中必定包含<span style='font-size:2em;color:orange'>HTTP首部</span>;

- 请求报文： 

  1. host: 表示访问的url /uri   [主机,主人,主持人]；
  2. user-agent:表示客户端程序信息；
  3. referer:表示是从哪个web页面发起的请求；

- 响应报文：

  1. location:表示原始url迁移，并提供新的url位置；返回状态码3XX（重定向）；
  2. server:告知客户端，服务器信息；

- 实体：

  1. content-type:text/html;charset='utf-8'
  2. content-language:zh-cn（中文中国）

- cookie:

  1. 服务器像客户端发送cookie
  2. 浏览器将cookie保存
  3. 之后每次http请求浏览器都会将cookie发送给服务器端

#####Chapter four: 返回结果的HTTP状态码

![](D:\Users\wbluo\Desktop\web html+css\status.png)

- 状态码：响应报文返回给客户端的结果状态；  4： 客户端问题；5：服务端问题；

##### Chapter five：构建web 内容技术

​     平时我们浏览的web 页面几乎全是HTML写成的。由	HTML构成的文本经过浏览器的解析、渲染后，呈现出来的结果就是web页面。

##### Chapter six：web的攻击技术

![](D:\Users\wbluo\Desktop\web html+css\attack.png)



------

##                                                                       <strong style='color:lightblue;text-align:center;'>                                                                           图解TCP_IP</strong>

------

#### Chapter one :网络基础知识

​    协议：计算机与计算机之间通过网络<strong><u>实现通信</u></strong>时事先达成的一种约定；只有两者采用相同协议才能相互通信；如同人与人之间沟通一样；

##### 1.6OSI七层通信

1. 应用层-->表示层-->会话层-->传输层-->网络层-->数据链路层-->物理层

    示例：

   应用层（“您好”）--> 表示层（将数据从某个计算机特定的数据格式 转化为 网络通用的标准数据格式)-->

   会话层（采用何种连接方式、何时建立连接、何时发送数据）-->传输层（建立连接或断开连接处理，创建逻辑上的通

   信连接）-->网络层（数据通信处理）-->

##### 1.7传输类型分类

​                             ![](D:\Users\wbluo\Desktop\web html+css\orient_1.png)

​     类似打电话一样： 拨打电话，对方接听才能创建连接，方能对话；

​                               ![](D:\Users\wbluo\Desktop\web html+css\orient_2.png)

​    类似快递员投递，只需要邮寄地址就行，不管对方是否收到；

#####1.7.2电路交换与分组交换

​              ![](D:\Users\wbluo\Desktop\web html+css\e_change.png)

​             电路交换缺陷：电路条数限制了终端通信个数 ；其他通信终端会等待资源释放；     

![](D:\Users\wbluo\Desktop\web html+css\group_CHANGE.png)

​            分组交换：解决率电路交换弊端，实现一条通信线路多个终端共享通讯作用；

1. 将数据集拆分成多个数据包，按照一定的顺序排列之后分别发送；然后每个数据包首部写入发送端和接收端mac地址，告知每个数据包目的地；

##### 1.8地址层次性

1. mac地址是由制造商针对每块网卡指定编号；

      网卡是硬件，网卡驱动是软件；任何硬件设备都需要驱动，从而使cpu 能控制硬件；就类似汽车与汽油；  

2. Ip地址是由网络号+主机号；

##### 1.9网络构成要素

1. 路由器

2. 网关

      在两个无法通信设备之间，将数据进行翻译；

      比如：手机邮件和互联网邮件之间的转化服务

   有时候两者无法通信，是因为他们在表示层和应用层中的电子邮件协议互不相同导致的；

   于是网关通过读完各种不同的协议后，再将数据发送出去；

![](D:\Users\wbluo\Desktop\web html+css\gateway.png)

   在使用www时，为了控制流量以及出于安全考虑，有时会使用代理服务器（proxy  server) ，这种代理服务器也是一种网关，称为应用网关；

3. 

#### Chapter two:TCP/IP基础知识

##### 2.1互联网的扩张

​    基于tcp/ip而形成的世界性范围网络---互联网诞生；

   

 

