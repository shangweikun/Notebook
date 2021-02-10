# RestEasy

官方文档：https://docs.jboss.org/resteasy/docs/3.0.4.Final/userguide/html_single/index.html

官方API：https://docs.jboss.org/resteasy/docs/3.1.0.Final/javadocs/overview-summary.html



RESTEasy 项目是 JAX-RS 的一个实现，集成的一些亮点：

- 不需要配置文件，只要把JARs文件放到类路径里面，添加 @Path 标注就可以了。
- 完全的把 RESTEeasy 配置作为Seam 组件来看待。
- HTTP 请求由Seam来提供，不需要一个额外的Servlet。
- Resources 和providers可以作为 Seam components (JavaBean or EJB)，具有全面的Seam injection,lifecycle, interception, 等功能支持。
- 支持在客户端与服务器端自动实现GZIP解压缩。



## JAX-RS规范

JAX-RS: Java API for RESTful Web Services是一个Java编程语言的应用程序接口,支持按照 表象化状态转变 (REST)架构风格创建Web服务Web服务[[1\]](http://zh.wikipedia.org/wiki/JAX-RS#cite_note-0).

 JAX-RS使用了Java SE 5引入的Java 标注来简化Web服务客户端和服务端的开发和部署。

****

***规范内容***

JAX-RS提供了一些标注将一个资源类，一个POJOJava类，封装为Web资源。标注包括：

@Path，标注资源类或方法的相对路径

@GET，@PUT，@POST，@DELETE，标注方法是用的HTTP请求的类型

@Produces，标注返回的MIME媒体类型

@Consumes，标注可接受请求的MIME媒体类型

@PathParam，@QueryParam，@HeaderParam，@CookieParam，@MatrixParam，@FormParam,分别标注方法的参数来自于HTTP请求的不同位置，例如@PathParam来自于URL的路径，@QueryParam来自于URL的查询参数，@HeaderParam来自于HTTP请求的头信息，@CookieParam来自于HTTP请求的Cookie。



## 使用说明

参考链接[2]

###与Spring集成

```xml
<web-app>

   <display-name>Archetype Created Web Application</display-name>

   <listener>

      <listener-class>org.jboss.resteasy.plugins.server.servlet.ResteasyBootstrap</listener-class>

   </listener>

   <listener>

      <listener-class>org.jboss.resteasy.plugins.spring.SpringContextLoaderListener</listener-class>

   </listener>

   <servlet>

      <servlet-name>Resteasy</servlet-name>

      <servlet-class>org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher</servlet-class>

   </servlet>

   <servlet-mapping>

      <servlet-name>Resteasy</servlet-name>

      <url-pattern>/*</url-pattern>

   </servlet-mapping>

</web-app>
```



###**使用@Path,@Get,@Post等标注**

```java
@Path("/library")

public class Library {

   @GET

   @Path("/books")

   public String getBooks() {...}

   @GET

   @Path("/book/{isbn}")

   public String getBook(@PathParam("isbn") String id) {

      // search my database and get a string representation and return it

   }

   @PUT

   @Path("/book/{isbn}")

   public void addBook(@PathParam("isbn") String id, @QueryParam("name") String name) {...}

   @DELETE

   @Path("/book/{id}")

   public void removeBook(@PathParam("id") String id {...}

```

#### @Path

@Path不仅仅接收简单的路径表达式,也可以使用正则表达式:

```java
@Path("/resources)

public class MyResource {

   @GET

   @Path("{var:.*}/stuff")

   public String get() {...}

}
```

如下操作就能获得该资源

GET /resources/stuff

GET /resources/foo/stuff

GET /resources/on/and/on/stuff

表达式的格式为:

***"{" variable-name [ ":" regular-expression ] "}"\***

***正则表达式部分是可选的,当未提供时,则会匹配一个默认的表达式\******"([]\*)"\***



```java
@Path("/resources/{var}/stuff")
```

会匹配如下路径

GET /resources/foo/stuff

GET /resources/bar/stuff

下面的则不会匹配

GET /resources/a/bunch/of/stuff



#### @PathParam高级使用

允许指定一个或多个路径参数在一个URI段中。例如：

1. @Path("/aaa{param}bbb")

"/aaa111bbb"会匹配,param=111

1. @Path("/{name}-{zip}")

"/bill-02115" 会匹配,name=bill,zip=02115

1. @Path("/foo{name}-{zip}bar")

"/foobill-02115bar " 会匹配,name=bill,zip=02115

正则表达式方式例子:

```java
@GET
@Path("/aaa{param:b+}/{many:.\*}/stuff")
public String getIt(@PathParam("param") String bs, @PathParam("many") String many) {...}
```

GET /aaabb/some/stuff bs=bb,many=some

GET /aaab/a/lot/of/stuff bs= b,many=a/lot/of



####@QueryParam

@QueryParam这个标注是给通过?的方式传参获得参数值的,如:

GET /books?num=5&index=1

```java
@GET

   public String getBooks(@QueryParam("num") int num,@QueryParam("index") int index) {

   ...

   }
```



####@HeaderParam

这个标注时用来获得保存在HttpRequest头里面的参数信息的,如:



####@PUT

  ```java
@PUT
public void put(@HeaderParam("Content-Type") MediaType contentType, ...)
  ```

这里同上面的@HeaderParam,参数类型可以是任意类型



####@CookieParam

用来获取保存在Cookie里面的参数,如:

```java
@GET

   public String getBooks(@CookieParam("sessionid") int id) {

   ...

   }
```



#### @FormParam

用来获取Form中的参数值,如:

页面代码:

```html
<form method="POST" action="/resources/service">

First name: 

<input type="text" name="firstname">

<br>

Middle name: 

<input type="text" name="middlename">

Last name: 

<input type="text" name="lastname">

</form>
```

后台代码:

```java
@Path("/")

public class NameRegistry {

   @Path("/resources/service")

   @POST

   public void addName(@FormParam("firstname") String first, @FormParam("lastname") String last) {...}
```

标注了@FormParam,会把表达里面的值自动映射到方法的参数上去.



如果要取得Form里面的所有属性,可以通过在方法上增加一个MultivaluedMap<String, String> form这样的对象来获得,如下:

```java
@Path("/resources/service")

   @POST

   public void addName(@FormParam("firstname") String first, @FormParam("lastname") String last,MultivaluedMap<String, String> form) {...}
```



####@Form

上面已经说的几种标注都是一个属性对应一个参数的,那么如果属性多了,定义的方法就会变得不好阅读,此时最好有个东西能够把上面的几种自动标注自动封装成一个对象,@Form这个标注就是用来实现这个功能的,如:

```java
public class MyForm {

   @FormParam("stuff")

   private int stuff;

   @HeaderParam("myHeader")

   private String header;

   @PathParam("foo")

   public void setFoo(String foo) {...}

}

@POST

@Path("/myservice")

public void post(@Form MyForm form) {...}
```





#### **@DefaultValue**

在以上标注使用的时候,有些参数值在没有值的情况下如果需要有默认值,则使用这个标注,如:

```java
@GET

   public String getBooks(@QueryParam("num") @DefaultValue("10") int num) {...}
```



####满足JAX-RS规范的 Resource Locators和子资源

资源处理类定义的某个方法可以处理某个请求的一部分,剩余部分由子资源处理类来处理,如:

```java
@Path("/")

public class ShoppingStore {

   @Path("/customers/{id}")

   public Customer getCustomer(@PathParam("id") int id) {

      Customer cust = ...; // Find a customer object

      return cust;

   }

}

public class Customer {

    @GET

    public String get() {...}

    @Path("/address")

    public String getAddress() {...}

}
```

当我们发起GET /customer/123这样的请求的时候,程序会先调用 ShoppingStore的 getCustomer这个方法,然后接着调用 Customer里面的 get方法

当我们发起GET /customer/123/address这样的请求的时候,程序会先调用 ShoppingStore的 getCustomer这个方法,然后接着调用 Customer里面的 getAddress 方法

###JAX-RS Content Negotiation

####@Consumes

我们从页面提交数据到后台的时候,数据的类型可以是text的,xml的,json的,但是我们在请求资源的时候想要请求到同一个资源路径上面去,此时怎么来区分处理呢?使用@Consumes标注,下面的例子将说明:

```java
 @Consumes("text/*")

         @Path("/library")

         public class Library {

         @POST

         public String stringBook(String book) {...}

         @Consumes("text/xml")

         @POST

         public String jaxbBook(Book book) {...}

```

当客户端发起请求的时候,系统会先找到所有匹配路径的方法,然后根据content-type找到具体的处理方法,比如:

当客户端发起请求的时候,系统会先找到所有匹配路径的方法,然后根据content-type找到具体的处理方法,比如:

 ***POST /library\***

***content-type: text/plain\***

就会执行上面的 stringBook这个方法,因为这个方法上面没有标注@ Consumes,程序找了所有的方法没有找到标注 @ Consumes(“text/plain”)这个类型的,所以就执行这个方法了.如果请求的content-type=xml,比如:

***POST /library\***

***content-type:text/xml\***

此时就会执行 jaxbBook这个方法



#### **@Produces**

当服务器端实行完成相关的逻辑需要返回对象的时候,程序会根据@Produces返回相应的对象类型

```java
@Produces("text/*")

         @Path("/library")

         public class Library {

         @GET

         @Produces("application/json")

         public String getJSON() {...}

         @GET

         public String get() {...}

```

如果客户端发起如下请求

​     ***GET /library\***

那么则会调用到get方法并且返回的格式是json类型的

***这些标注能不能写多个呢?***

​			答案是可以的,但是系统只认第一个



#### **生成 JavaScript API**

RESTEasy能够生成JavaScript API使用AJAX来执行 JAX-RS操作,比如:

```java
@Path("orders")

public interface Orders {

 @Path("{id}")

 @GET

 public String getOrder(@PathParam("id") String id){

  return "Hello "+id;

 }

}
```

以上代码可以在js里面通过

```javascript
var order = Orders.getOrder({id: 23});
```

这种方式来调用,很酷吧,这里应该是跟Google的一项技术类似的Java代码可以通过js方式来调用



#### **通过JavaApi调用资源**

废话不多说，直接上代码：

```java
ClientRequest request = new ClientRequest("http://localhost:8080/rest/services/demoservice/child/22222");

// request.header("custom-header", "value");

// We're posting XML and a JAXB object

// request.body("application/xml", someJaxb);

// we're expecting a String back

ClientResponse<Object> response = request.get(Object.class);

if (response.getStatus() == 200) // OK!

{

Object str = response.getEntity();

System.out.println(str);

}
```

**把资源当做一个标准servlet接收处理方法**

我们可以把一个资源url当做一个接收servlet请求的处理类或者处理方法



#### 参数转化处理

**对@PathParam, @QueryParam, @MatrixParam, @FormParam, and @HeaderParam参数的处理**

* @PathParam, @QueryParam, @MatrixParam, @FormParam, and @HeaderParam标注传递的参数类型是String型的,对于这些参数我们的方法可能希望接收的参数是经过转换后的对象类型的参数,比如如下的方法:

```java
void put(@QueryParam("pojo")POJO q, @PathParam("pojo")POJO pp,@MatrixParam("pojo")POJO mp,@HeaderParam("pojo")POJO hp);
```

这里的Put方法需要的是一个Pojo类型的参数,但是@PathParam, @QueryParam, @MatrixParam, @FormParam, and @HeaderParam传递的都是String类型的,怎么是怎么转换为对象的呢?

可以使用***StringConverter***或者***StringParamUnmarshaller***

```java
package org.jboss.resteasy.spi;

public interface StringConverter<T>

{

   T fromString(String str);

   String toString(T value);

}

```

FromString就是你自己需要实现的如何把接收的String参数转换为Pojo类

toString方法是用来把Pojo对象转换为String

实现类如下:

```java
@Provider

      public static class POJOConverter implements StringConverter<POJO>

      {

         public POJO fromString(String str)

         {

            System.out.println("FROM STRNG: " + str);

            POJO pojo = new POJO();

            pojo.setName(str);

            return pojo;

         }

         public String toString(POJO value)

         {

            return value.getName();

         }

      }
```

现在已经能够把String参数转换为对象了,我们更进一步的想使用一些自定义的标注来做一些逻辑,比如说日期的格式化,就要使用下面的***StringParamUnmarshaller***

```java
StringParamUnmarshaller

package org.jboss.resteasy.spi;

public interface StringParameterUnmarshaller<T>

{

   void setAnnotations(Annotation[] annotations);

   T fromString(String str);

}
```

实现类

```java
public class DateFormatter implements StringParameterUnmarshaller<Date>

   {

      private SimpleDateFormat formatter;

      public void setAnnotations(Annotation[] annotations)

      {

         DateFormat format = FindAnnotation.findAnnotation(annotations, DateFormat.class);

         formatter = new SimpleDateFormat(format.value());

      }

      public Date fromString(String str)

      {

         try

         {

            return formatter.parse(str);

         }

         catch (ParseException e)

         {

            throw new RuntimeException(e);

         }

      }

   }
```

使用方式:

```java
@Path("/datetest")

   public class Service

   {

      @GET

      @Produces("text/plain")

      @Path("/{date}")

      public String get(@PathParam("date") @DateFormat("MM-dd-yyyy") Date date)

      {

         System.out.println(date);

         Calendar c = Calendar.getInstance();

         c.setTime(date);

         Assert.assertEquals(3, c.get(Calendar.MONTH));

         Assert.assertEquals(23, c.get(Calendar.DAY_OF_MONTH));

         Assert.assertEquals(1977, c.get(Calendar.YEAR));

         return date.toString();

      }

   }
```



在实际使用中,我们有些参数值并不是通过以上方式来传递的,比如说我们要对session进行操作,那么应该怎么办呢,resteasy并没有直接提供使用自定义标注的方法,所以我们可以使用以上的 StringParamUnmarshaller来变通的实现

首先定义自定义标注

```java
@Retention(RetentionPolicy.RUNTIME)

@StringParameterUnmarshallerBinder(SessionOperator.class)

public @interface Session {

				public String value();

}
```

***@StringParameterUnmarshallerBinder(SessionOperator.class)***

是用来指明这个自定义标注是哪个具体的类来处理, ***SessionOperator***这个类就是**Session**这个自定义标注的处理类



```java
public class SessionOperator implements StringParameterUnmarshaller{

      public void setAnnotations(Annotation[] annotations) {}

      public Object fromString(String str) {
          return null;
      }
}
```

***自我延伸后的想法💡***：是否可以通过该种方式，来获取Session中的对象呢？

```java
public class SessionOperator implements StringParameterUnmarshaller{
			
  		@Context 
			private HttpServletRequest httpRequest;
  
      public void setAnnotations(Annotation[] annotations) {}

      public Object fromString(String str) {
        	final HttpSession session = httpRequest.getSession();
        	/**
        					对Session进行特殊的操作，根据str，获取对应的session内容
        	*/
          return null;
      }
}
```





#表现层状态转换

参考：https://zh.wikipedia.org/wiki/%E8%A1%A8%E7%8E%B0%E5%B1%82%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2

（[英语](https://zh.wikipedia.org/wiki/英语)：**Representational State Transfer**，[缩写](https://zh.wikipedia.org/wiki/縮寫)：**REST**）是[Roy Thomas Fielding](https://zh.wikipedia.org/w/index.php?title=Roy_Thomas_Fielding&action=edit&redlink=1)博士于2000年在他的博士论文[[1\]](https://zh.wikipedia.org/wiki/表现层状态转换#cite_note-Fielding-Ch5-1)中提出来的一种[万维网](https://zh.wikipedia.org/wiki/万维网)[软件架构](https://zh.wikipedia.org/wiki/软件架构)风格，目的是便于不同软件/程序在网络（例如互联网）中互相传递信息。表现层状态转换是根基于[超文本传输协议（HTTP）](https://zh.wikipedia.org/wiki/超文本传输协议)之上而确定的一组约束和属性，是一种设计提供万维网络服务的[软件构建风格](https://zh.wikipedia.org/wiki/軟件架構)。符合或兼容于这种架构风格（简称为 REST 或 RESTful）的网络服务，允许客户端发出以[统一资源标识符](https://zh.wikipedia.org/wiki/统一资源标志符)访问和操作网络资源的请求，而与预先定义好的无状态操作集一致化。因此表现层状态转换提供了在互联网络的计算系统之间，彼此资源可交互使用的协作性质（interoperability）。相对于其它种类的网络服务，例如SOAP服务，则是以本身所定义的操作集，来访问网络上的资源。



## REST 关键原则

大部分对 REST 的介绍是以其正式的定义和背景作为开场的。但这儿且先按下不表，我先提出一个简单扼要的定义：REST 定义了应该如何正确地使用（这和大多数人的实际使用方式有很大不同）Web 标准，例如 HTTP 和 URI。如果你在设计应用程序时能坚持 REST 原则，那就预示着你将会得到一个使用了优质 Web 架构（这将让你受益）的系统。总之，五条关键原则列举如下：

- 为所有“事物”定义 ID
- 将所有事物链接在一起
- 使用标准方法
- 资源多重表述
- 无状态通信



###1～为所有“事物”定义 ID

* 每个事物都应该是可标识的，都应该拥有一个明显的 ID——在 Web 中，代表 ID 的统一概念是：URI。URI 构成了一个全局命名空间，使用 URI 标识你的关键资源意味着它们获得了一个唯一、全局的 ID；

* 对事物使用一致的命名规则（naming scheme）最主要的好处就是你不需要提出自己的规则——而是依靠某个已被定义，在全球范围中几乎完美运行，并且能被绝大多数人所理解的规则。

* 标识所有值得标识的事物，领会这个观念可以进一步引导你创造出在传统的应用程序设计中不常见的资源：一个流程或者流程步骤、一次销售、一次谈判、一份报价请求——这都是应该被标识的事物的示例。同样，这也会导致创建比非 RESTful 设计更多的持久化实体。



###2～将所有事物链接在一起

* 使用一个遵守专用命名规范的简单“id”属性作为链接，也是可行的——**但是仅限于应用环境之内**。使用 URI 表示链接的优雅之处在于，链接可以指向由不同应用、不同服务器甚至位于另一个大陆上的不同公司提供的资源——因为 URI 命名规范是全球标准，构成 Web 的所有资源都可以互联互通。

```java
class Resource {
    Resource(URI u);
    Response get();
    Response post(Request r);
    Response put(Request r);
    Response delete();
} 
```

* 由于所有资源使用了同样的接口，你可以依此使用 GET 方法检索一个**表述**（representation）——也就是对资源的描述。因为规范中定义了 GET 的语义，所以可以肯定当你调用它的时候不需要对后果负责——这就是为什么可以“安全”地调用此方法。GET 方法支持非常高效、成熟的缓存，所以在很多情况下，你甚至不需要向服务器发送请求。还可以肯定的是，GET 方法具有**幂等性**[译注：指多个相同请求返回相同的结果]——如果你发送了一个 GET 请求没有得到结果，你可能不知道原因是请求未能到达目的地，还是响应在反馈的途中丢失了。幂等性保证了你可以简单地再发送一次请求解决问题。幂等性同样适用于 PUT（基本的含义是“更新资源数据，如果资源不存在的话，则根据此 URI 创建一个新的资源”）和 DELETE（你完全可以一遍又一遍地操作它，直到得出结论——删除不存在的东西没有任何问题）方法。POST 方法，通常表示“创建一个新资源”，也能被用于调用任意过程，因而它既不安全也不具有幂等性。



在 RESTful HTTP 方式中，你将通过组成**HTTP 应用协议**的通用接口访问服务程序。你可能会想出像这样的方式：

![restful-标准方法图片](/Users/swk/Pictures/JavaWeb技术收集/restful-标准方法图片.jpg)



###3～资源多重表述

***理想的情况下***：

资源表述应该采用标准格式——如果客户程序对 HTTP 应用协议和一组数据格式都有所“了解”，那么它就可以用一种有意义的方式**与世界上任意一个 RESTful HTTP 应用交互**。

* 资源多重表述还有另外一种使用方式：你可以将应用的 Web UI 纳入到 Web API 中——毕竟，API 的设计通常是由 UI 可以提供的功能驱动的，而 UI 也是通过 API 执行动作的。将这两个任务合二为一带来了令人惊讶的好处，这使得使用者和调用程序都能得到更好的 Web 接口。



###4～无状态通信

* 服务器端不能保持除了单次请求之外的，任何与其通信的客户端的通信状态。这样做的最直接的理由就是可伸缩性—— 如果服务器需要保持客户端状态，那么大量的客户端交互会严重影响服务器的内存可用空间（footprint）。（注意，要做到无状态通信往往需要需要一些重新设计——不能简单地将一些 session 状态绑缚在 URI 上，然后就宣称这个应用是 RESTful。）





参考：

https://www.infoq.cn/article/rest-introduction/[1]

https://my.oschina.net/bigyuan/blog/57409[2]





##Rest请求处理链路

#### 一.Providers 详解

[javax.ws.rs.ext.Providers](https://link.jianshu.com?t=http://javax.ws.rs.ext.Providers) 是JAX-RS 2.0定义的一种辅助接口，其实现类用于辅助REST框架完成过滤和读写拦截的功能，可以使用@Provider 注解标注这些类。Providers接口一共定义了四个方法，分别用来获取MessageBodyReader，MessageBodyWriter，ExceptionMapper，ContextResolver

```java
package javax.ws.rs.ext;

import java.lang.annotation.Annotation;
import java.lang.reflect.Type;

import javax.ws.rs.core.MediaType;

public interface Providers {
    
    <T> MessageBodyReader<T> getMessageBodyReader(Class<T> type,
                                                  Type genericType, Annotation[] annotations, MediaType mediaType);

    <T> MessageBodyWriter<T> getMessageBodyWriter(Class<T> type,
                                                  Type genericType, Annotation[] annotations, MediaType mediaType);

    <T extends Throwable> ExceptionMapper<T> getExceptionMapper(Class<T> type);

    <T> ContextResolver<T> getContextResolver(Class<T> contextType,
                                              MediaType mediaType);
}
```

  

<img src="/Users/swk/Pictures/JavaWeb技术收集/RESTEasy请求路由图.jpg" alt="RESTEasy请求路由图" style="zoom:50%;" />

如上图，请求流程中存在三种角色，分别是：用户，REST客户端和REST服务器，请求始于请求的发送，止于调用Resonse的readEntity()方法
 （1）.用户请求提交数据，客户端接收请求，进入第一个扩展点：客户端请求过滤器 ClientRequestFilter 的filter()方法
 （2）.请求处理过滤完毕后，流程进入第二个扩展点：客户端写拦截器WriterInterceptor实现类的aroundWriterTo() 方法，实现对客户端序列化操作的拦截
 （3）.客户端消息体写处理器MessageBodyWriter 执行序列化，流程从客户端过渡到服务器端
 （4）.服务器接收请求，流程进入第三个扩展点：服务器前置请求过滤器ContainerRequestFilter实现类 的filter()方法
 （５）.过滤器处理完毕后，服务器根据请求匹配资源方法，如果匹配到相应的资源方法，流程进入第四个扩展点：服务器后置请求过滤器ContainerRequestFilter 实现类 的filter()  方法
 （6）.后置请求过滤器处理完毕后，力促进入第五个扩展点：服务器读拦截器ReaderInterceptor实现类 的aroundReadFrom() 方法，拦截服务器端反序列化操作
 （7）.服务器消息体读处理器MessageBodyReader 完成对客户端数据流的反序列化，服务器执行匹配的资源方法
 （8）.REST请求资源的处理完毕后，流程进入第六个扩展点：服务器响应过滤器 ContainerResponseFilter 实现类 的filter() 方法
 （9）.过滤器处理完毕后，流程进入第七个扩展点：服务器写拦截器WriterInterceptor实现类 的aroundWriterTo() 方法，实现对服务器端序列化到客户端这个操作的拦截
 （10）.服务器消息体写处理器MessageBodyWriter 执行序列化，流程返回到客户端一侧
 （11）.客户端接收响应，流程进入第八个扩展点：客户端响应过滤器ClientResponseFilter 实现类 的filter() 方法
 （12）.过滤处理完毕后，客户端响应实例response 返回到用户一侧，用户执行response.readEntity(),流程进入第九个扩展点：客户端拦截器ReaderInterceptor实现类 的aroundReadFrom() 方法，对客户端反序列化进行拦截
 （13）.客服端消息体读处理器MessageBodyReader 执行反序列化，将Java类型的对象最终作为readENtity()方法的返回值



鸣谢：

https://www.jianshu.com/p/e250fdb794f4