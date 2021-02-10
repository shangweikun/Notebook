# WEB学习

<h1>test
</h1>

<h6>
  test
</h6>

h1 - h6 标题的切换

<br/>

浏览器会移除*源代码中*多余的空格和空行



```html
<html>

<body style="background-color:yellow">
<h2 style="background-color:red">This is a heading</h2>
<p style="background-color:green">This is a paragraph.</p>
</body>

</html>

<html>

<body>
<h1 style="font-family:verdana">A heading</h1>
<p style="font-family:arial;color:red;font-size:20px;">A paragraph.</p>
</body>

</html>
```

style属性 样式



<pre/>标签

title属性 缩略词



##JSP(JavaServer Pages)

https://www.cnblogs.com/zempty/p/4199750.html

官方文档：https://www.oracle.com/java/technologies/jspt.html



## Axis组件（已经废弃，最后更新为2006年）

https://www.webforefront.com/about/danielrubio/articles/ostg/javawebservices.html





# Web基础支持

web服务器：Apache，IIS，Nginx；

中间件：MQ，web应用服务器等等；

Web应用服务器：

​			WebLogic和WebSphere重量级产品；

​			Tomcat、jetty这样的web containner 再加上第三方的框架(spring,hibernate等)来构建自己的Application Server；

web容器（Servlet容器）：Jetty，Tomcat，IIS中的功能；

 EJB 容器：JBoss；

~~http服务器~~

参考：

https://developer.aliyun.com/article/42059

***\*https://www.cnblogs.com/vipyoumay/p/7455431.html\****



何为容器： 

容器是一种服务调用规范框架，J2EE 大量运用了容器和组件技术来构建分层的企业级应用。在 J2EE 规范中，相应的有 WEB Container 和 EJB Container 等。

参考：https://cloud.tencent.com/developer/article/1490621



什么是web容器？[A **web container** (also known as a servlet container;[[1\]](https://en.wikipedia.org/wiki/Web_container#cite_note-1) and compare "webcontainer"[[2\]](https://en.wikipedia.org/wiki/Web_container#cite_note-2))]

https://www.educative.io/edpresso/what-is-a-web-container

https://en.wikipedia.org/wiki/Web_container



web服务器和应用服务器区别？

https://www.ibm.com/cloud/learn/web-server-vs-application-server



什么是应用服务器？

https://en.wikipedia.org/wiki/Application_server



引出：

WampServer是一款由法国人开发的Apache Web服务器、[PHP](https://baike.baidu.com/item/PHP/9337)解释器以及[MySQL数据库](https://baike.baidu.com/item/MySQL数据库/10991669)的整合软件包。免去了开发人员将时间花费在繁琐的配置环境过程，从而腾出更多精力去做开发。



企业级web容器：weblogic（Oracle）｜webSphere（IBM）

主流的开源web容器（java 应用服务器）：tomcat（jsp和servlet的容器）｜Jetty（Eclipse）｜undertow（RedHat）

Http服务器：Apache｜Nginx（也是一个反向代理软件）

![20190407101654161](/Users/swk/Pictures/20190407101654161.png)



## Jetty

jetty本身仅支持http协议，并不支持静态资源的处理；

ps：通过编写Service可以实现该功能：https://juejin.im/post/6844903759227650055

![Jetty架构图](/Users/swk/Pictures/JavaWeb技术收集/Jetty架构图.png)



https://zh.wikipedia.org/wiki/Jetty



## Tomcat

Tomcat本身也可以进行静态资源的解析；

![Tomcat架构图](/Users/swk/Pictures/JavaWeb技术收集/Tomcat架构图.gif)



作者：meepo
链接：https://www.zhihu.com/question/57400909/answer/154753720
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



Tomcat访问所有的资源，都是用Servlet来实现的。

在Tomcat看来，资源分3种

1. 静态资源，如css,html,js,jpg,png等

2. Servlet 

3. JSP

对于静态资源，Tomcat最后会交由一个叫做DefaultServlet的类来处理

对于Servlet ，Tomcat最后会交由一个叫做 InvokerServlet的类来处理

对于JSP，Tomcat最后会交由一个叫做JspServlet的类来处理

所以Tomcat又叫Servlet容器嘛，什么都交给Servlet来处理。

那么什么时候调用哪个Servlet呢？ 有一个类叫做org.apache.tomcat.util.http.mapper.Mapper，它一共进行了7个大的规则判断，第7个，就是判断是否是该用DefaultServlet。

简单地说。。。先看是不是servlet,然后看是不是jsp，如果都不是，那么就是你DefaultServlet的活儿了。



![img](https://pic4.zhimg.com/50/v2-87009c876e6bb2626d721bdef7b5aa3b_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-87009c876e6bb2626d721bdef7b5aa3b_1440w.jpg?source=1940ef5c)



到了DefaultServlet之后，就是一个普通的HttpServlet了，doPost方法会交由doGet处理：



![img](https://pic4.zhimg.com/50/v2-ed7bc2e94d38d0826614a70bb3296394_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-ed7bc2e94d38d0826614a70bb3296394_1440w.jpg?source=1940ef5c)

doGet又交由一个叫做 serveResource的方法处理

![img](https://pic4.zhimg.com/50/v2-757a1359dccad78d871579c1698f718c_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-757a1359dccad78d871579c1698f718c_1440w.jpg?source=1940ef5c)

在serveResource方法里又瞎搞八搞了许多事情，最后在一个叫做copy()方法里，把静态资源对应的输入流 读取出来，扔到了输出流里，这样你的浏览器就看到数据了。

![img](https://pic4.zhimg.com/50/v2-acb790053acec8bf34685ba132ff6540_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-acb790053acec8bf34685ba132ff6540_1440w.jpg?source=1940ef5c)



PS：如果没有前面的静态服务器，所有的资源都用tomcat的静态资源servlet处理，都会占掉了一个tomcat的线程池的一个线程，这样tomcat的性能就会受到非常大的影响。



## Jboss

JBoss是一个管理EJB的容器和服务器，支持EJB 1.1、EJB 2.0和EJB3的规范。

2014年11月20日，JBoss更名为WildFly。

具体知识参考：

https://zh.wikipedia.org/wiki/WildFly

https://www.wildfly.org/



### Undertow





官方网站：https://undertow.io/



##Weblogic

weblogic是j2ee的应用服务器（application server），包括ejb ,jsp,servlet,jms等等，全能型的。是商业软件里排名第一的容器（JSP、servlet、EJB等），并提供其他如JAVA编辑等工具，是一个综合的开发及运行环境。

***WebLogic***包含J2EE Container，***Tomcat***只能算Web Container，是官方指定的JSP&Servlet容器。只实现了JSP/Servlet的相关规范，不支持EJB（硬伤啊）!不过Tomcat配合jboss和apache可以实现j2ee应用服务器功能

https://blog.csdn.net/u013573133/article/details/23379565



***在UNⅨ和LINUX平台下使用最广泛的免费HTTP服务器是Apache和Nginx服务器，而Windows平台NT/2000/2003使用ⅡS的WEB服务器。***



## Apache HTTP Server

官方网站：https://httpd.apache.org/



***Apache(音译为[阿帕奇](https://baike.baidu.com/item/阿帕奇/374191))是世界使用排名第一的Web[服务器](https://baike.baidu.com/item/服务器)软件。一般是作为静态资源的服务器，和Tomcat一起使用***



HTTP服务器本质上也是一种应用程序——它通常运行在服务器之上，绑定服务器的IP地址并监听某一个tcp端口来接收并处理HTTP请求，这样客户端（一般来说是IE, Firefox，Chrome这样的浏览器）就能够通过HTTP协议来获取服务器上的网页（HTML格式）、文档（PDF格式）、音频（MP4格式）、视频（MOV格式）等等资源。下图描述的就是这一过程：



作者：知乎用户
链接：https://www.zhihu.com/question/32212996/answer/87524617
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

![http请求格式](/Users/swk/Pictures/JavaWeb技术收集/http请求格式.jpg)





## Nginx



nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy server, originally written by [Igor Sysoev](http://sysoev.ru/en/).

https://nginx.org/en/



## IIS(Internet Information Services)

官方网站：https://www.iis.net/

互联网信息服务（Internet Information Services,简称IIS），是由微软公司提供的基于运行Microsoft [Windows](https://baike.baidu.com/item/Windows/165458)的互联网基本服务。





# RestFul

https://www.infoq.cn/article/2007/07/dlee-fielding-rest/

https://www.infoq.cn/minibook/dissertation-rest-cn

论文地址

英文版：https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm

中文版： [FieldingREST.pdf](../../../Books/FieldingREST.pdf) ｜ [架构风格与基于网络的软件架构设计.pdf](../../../Books/架构风格与基于网络的软件架构设计.pdf) 





## 跨域问题

同源策略：同源策略是浏览器的一个安全限制，从一个源加载的文档或者脚本默认不能访问另一个源的资源。

### 源：如果两个页面（接口）的协议，端口或者域名都相同，那么两个页面就有相同的源。

参考：https://www.jianshu.com/p/d43e73efefe1



跨域解决方案：

JSONP，CORS等

参考：

https://bibichuan.github.io/posts/48aaa7c5.html

https://www.jianshu.com/p/a46e62f9ad1c

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS



## 重定向

微信重定向的思考：

***重定向：浏览器在接收到重定向响应的时候，会采用该响应提供的新的 URL ，并立即进行加载；大多数情况下，除了会有一小部分性能损失之外，重定向操作对于用户来说是不可见的。***

![img](https://mdn.mozillademos.org/files/13785/HTTPRedirect.png)

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Redirections

