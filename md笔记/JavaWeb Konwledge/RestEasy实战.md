# 实战篇

* 普通web项目的搭建工作

参考链接：

https://www.jianshu.com/p/4a4ff98bef0e

https://developer.jboss.org/docs/DOC-13709



## 新建web项目 by ideal

本次使用IntelliJ IDEA创建一个可运行的RESTEasy的web项目，使用环境如下：

* 电脑系统：MAC OS 10.15.6 (19G2021)
* IDE环境：IDEA 2020.2.3
* Severlet容器：Tomcat 9.0.38
* JDK版本：JDK14



###1、选择 `New Project` 一个IntelliJ的工程

![截屏2020-10-12 下午10.08.37](/Users/swk/ScreenPictures/20201012/截屏2020-10-12 下午10.08.37.png)

-----

###2、新建一个`Java Enterprise`的 Java工程，点击`Next`

![](/Users/swk/ScreenPictures/20201012/截屏2020-10-12 上午8.17.42.png)

---

###3、点击选中`Implementations`中的`RESTEasy`的框架，点击`Next`

![截屏2020-10-12 上午8.18.18](/Users/swk/ScreenPictures/20201012/截屏2020-10-12 上午8.18.18.png)

---

###4、使用命令编译项目（强迫症患者，必须要看着滚动才舒畅）Command：`mvn compile -e`

![截屏2020-10-12 上午8.19.39](/Users/swk/ScreenPictures/20201012/截屏2020-10-12 上午8.19.39.png)

---

###5、新建`Resource`类和`Application`类

代码目录层级结构如下：

<img src="/Users/swk/Library/Application Support/typora-user-images/image-20201012222644996.png" alt="image-20201012222644996" style="zoom:50%;" />

* Resource资源类是restful规范中对外提供服务的入口；

代码示例如下：

```java
package org.jboss.send.demo;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.core.Response;

@Path("/")
public class ResourceDemo {

	@GET
	@Path("hello")
	public Response getWord() {
		System.out.println("ok");
		return Response.status(200).entity("HelloWorld").build();
	}
}
```



* Application资源处理类是用来加载对应的Resource资源；

代码示例如下：

```java
package org.jboss.send.demo;

import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;
import java.util.HashSet;
import java.util.Set;

@ApplicationPath("/rest")
public class MyApplication extends Application {

	Set<Object> singletons = new HashSet<>();

	public MyApplication() {
		super();
		singletons.add(new ResourceDemo());
	}

	@Override
	public Set<Object> getSingletons() {
		return singletons;
	}
}
```

---

###6、其他配置展示

其他配置均未调整，因为在依赖中有 `resteasy-servlet-initializer` 可自动加载配置

```xml
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-servlet-initializer</artifactId>
    <version>4.5.8.Final</version>
</dependency>
```

对于本次项目，需要关注的配置参数主要是`*pom.xml`(maven应用)及`*web.xml`(web应用)

***pom.xml*** 如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>RestEasyDemo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>RestEasyDemo</name>
    <packaging>war</packaging>

    <properties>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.source>1.8</maven.compiler.source>
        <junit.version>5.6.2</junit.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.jboss.resteasy</groupId>
            <artifactId>resteasy-servlet-initializer</artifactId>
            <version>4.5.8.Final</version>
        </dependency>
        <dependency>
            <groupId>org.jboss.resteasy</groupId>
            <artifactId>resteasy-jackson2-provider</artifactId>
            <version>4.5.8.Final</version>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.3.0</version>
            </plugin>
        </plugins>
    </build>
</project>
```



***web.xml*** 如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
</web-app>
```

---

###7、Servelet容器配置

本次应用使用Tomcat容器，并未选择Red Hat的本家产品Undertow，Ideal中配置Tomcat容器步骤如下：

​	1).点击 `Run` 弹出下拉框后，选择 `Edit Configurations`

<img src="/Users/swk/ScreenPictures/20201012/截屏2020-10-12 下午10.39.33.png" alt="截屏2020-10-12 下午10.39.33" style="zoom:50%;" />

​	

​	2). 点击 `+` 新增 `Tomcat Servlet` 的容器

<img src="/Users/swk/ScreenPictures/20201012/截屏2020-10-12 下午11.01.21.png" alt="截屏2020-10-12 下午11.01.21" style="zoom:50%;" />

​	3).点击 `CONFIGURE...` 新增Tomcat 的 `Application server` (图中已经新增完毕)

![截屏2020-10-12 下午11.01.32](/Users/swk/ScreenPictures/20201012/截屏2020-10-12 下午11.01.32.png)

​	4).点击 `Deployment` 新增部署项目，修改 `Application Context` 为  `/ `

![截屏2020-10-13 上午7.33.30](/Users/swk/ScreenPictures/20201012/截屏2020-10-13 上午7.33.30.png)

---

### 8、结果展示

启动服务后，在浏览器输入以下路径：

```
http://localhost:8080/rest/hello
```

结果展示页面：

![截屏2020-10-13 上午8.28.10](/Users/swk/ScreenPictures/20201012/截屏2020-10-13 上午8.28.10.png)







## 常见问题

* @POST注解，可以接受一个`raw`模式参数为 - `Map`或者`bo`

  Poin平台的使用，应该是通过反射方式，动态实现的，需要仔细查看代码。

  

* 当方法存在返回时：`@Produces({})`使用，会导致异常；不使用`@Produces`注解则无异常

 ```
11-Oct-2020 20:23:09.560 严重 [http-nio-8080-exec-8] org.apache.catalina.core.StandardWrapperValve.invoke 在路径为[]的上下文中，servlet[Resteasy]的Servlet.service()引发异常
	org.jboss.resteasy.spi.UnhandledException: java.lang.NullPointerException
		at org.jboss.resteasy.core.ExceptionHandler.handleException(ExceptionHandler.java:381)
		at org.jboss.resteasy.core.SynchronousDispatcher.writeException(SynchronousDispatcher.java:216)
		at org.jboss.resteasy.core.SynchronousDispatcher.writeResponse(SynchronousDispatcher.java:610)
		at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:520)
		at org.jboss.resteasy.core.SynchronousDispatcher.lambda$invoke$4(SynchronousDispatcher.java:259)
		at org.jboss.resteasy.core.SynchronousDispatcher.lambda$preprocess$0(SynchronousDispatcher.java:160)
		at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:362)
		at org.jboss.resteasy.core.SynchronousDispatcher.preprocess(SynchronousDispatcher.java:163)
		at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:245)
		at org.jboss.resteasy.plugins.server.servlet.ServletContainerDispatcher.service(ServletContainerDispatcher.java:249)
		at org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher.service(HttpServletDispatcher.java:60)
		at org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher.service(HttpServletDispatcher.java:55)
		at javax.servlet.http.HttpServlet.service(HttpServlet.java:733)
		at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231)
		at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
		at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
		at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
		at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
		at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:202)
		at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)
		at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541)
		at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:143)
		at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)
		at org.apache.catalina.valves.AbstractAccessLogValve.invoke(AbstractAccessLogValve.java:690)
		at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)
		at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343)
		at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:374)
		at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)
		at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:868)
		at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1590)
		at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)
		at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
		at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
		at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
		at java.lang.Thread.run(Thread.java:748)
	Caused by: java.lang.NullPointerException
		at org.jboss.resteasy.core.ServerResponseWriter.getDefaultContentType(ServerResponseWriter.java:301)
		at org.jboss.resteasy.core.ServerResponseWriter.getResponseMediaType(ServerResponseWriter.java:203)
		at org.jboss.resteasy.core.ServerResponseWriter.setResponseMediaType(ServerResponseWriter.java:189)
		at org.jboss.resteasy.core.ServerResponseWriter.writeNomapResponse(ServerResponseWriter.java:99)
		at org.jboss.resteasy.core.ServerResponseWriter.writeNomapResponse(ServerResponseWriter.java:74)
		at org.jboss.resteasy.core.SynchronousDispatcher.writeResponse(SynchronousDispatcher.java:590)
		... 32 more

 ```



* 新建web maven项目无路径问题｜src无目录问题：

  创建时增加设置

  ```php
  archetype=Internal
  ```

参考：https://blog.csdn.net/lk142500/article/details/88782116

 https://www.cnblogs.com/xuanwei-qingfeng/p/8473450.html





## ExceptionMapper拦截器

jboss对于拦截器的选择

### 优先级确定

```java
ExceptionHandle.executeExceptionMapper{
  ...
    
  this.providerFactory.getExceptionMapperForClass(causeClass);
 
}


```



***ResteasyProviderFactoryImpl***中的add方法实现对***ExceptionMapper***的实现类的排序

```java
private void addExceptionMapper(ExceptionMapper provider, Class providerClass, boolean isBuiltin) {
    if (providerClass.isSynthetic()) {
        providerClass = providerClass.getSuperclass();
    }
    Type exceptionType = Types.getActualTypeArgumentsOfAnInterface(providerClass, ExceptionMapper.class)[0];
    Utils.injectProperties(this, providerClass, provider);
    Class<?> exceptionClass = Types.getRawType(exceptionType);
    if (!Throwable.class.isAssignableFrom(exceptionClass)) {
        throw new RuntimeException(Messages.MESSAGES.incorrectTypeParameterExceptionMapper());
    } else {
        if (this.sortedExceptionMappers == null) {
            this.sortedExceptionMappers = new CopyOnWriteMap(this.parent.getSortedExceptionMappers());
        }
        int priority = Utils.getPriority((Integer)null, (Map)null, ExceptionMapper.class, providerClass);
        SortedKey<ExceptionMapper> candidateExceptionMapper = new SortedKey((Class)null, provider, providerClass, priority, isBuiltin);
        SortedKey registeredExceptionMapper;
      //candidate替换registered value比较
        if ((registeredExceptionMapper = (SortedKey)this.sortedExceptionMappers.get(exceptionClass)) == null || candidateExceptionMapper.compareTo(registeredExceptionMapper) <= 0) {
            this.sortedExceptionMappers.put(exceptionClass, candidateExceptionMapper);
        }
    }
}
```

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.jboss.resteasy.core.providerfactory;

import org.jboss.resteasy.core.MediaTypeMap.Typed;
import org.jboss.resteasy.spi.util.Types;

public class SortedKey<T> implements Comparable<SortedKey<T>>, Typed {
    private final T obj;
    private final boolean isBuiltin;
    private final Class<?> template;
    private final int priority;

    public SortedKey(Class<?> intf, T reader, Class<?> readerClass, int priority, boolean isBuiltin) {
        this.obj = reader;
        Class<?> t = Types.getTemplateParameterOfInterface(readerClass, intf);
        this.template = t != null ? t : Object.class;
        this.priority = priority;
        this.isBuiltin = isBuiltin;
    }

    public SortedKey(Class<?> intf, T reader, Class<?> readerClass, boolean isBuiltin) {
        this(intf, reader, readerClass, 5000, isBuiltin);
    }

    public SortedKey(Class<?> intf, T reader, Class<?> readerClass) {
        this(intf, reader, readerClass, 5000, false);
    }

    public int compareTo(SortedKey<T> tMessageBodyKey) {
        if (this == tMessageBodyKey) {
            return 0;
        } else {
            if (this.isBuiltin == tMessageBodyKey.isBuiltin) {
                if (this.priority < tMessageBodyKey.priority) {
                    return -1;
                }

                if (this.priority == tMessageBodyKey.priority) {
                    return 0;
                }

                if (this.priority > tMessageBodyKey.priority) {
                    return 1;
                }
            }

            return this.isBuiltin ? 1 : -1;
        }
    }

    public Class<?> getType() {
        return this.template;
    }

    public T getObj() {
        return this.obj;
    }
}
```

优先级小，优先程度高。











##rs规范中两种拦截器

javax.ws.rs.ext.WriterInterceptor：写拦截器，可以在其中对于响应实体进行拦截操作；
javax.ws.rs.ext.ReaderInterceptor：读拦截器，可以在其中对于请求实体进行拦截操作；

