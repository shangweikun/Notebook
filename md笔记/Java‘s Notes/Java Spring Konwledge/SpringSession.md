### Spring Session

Spring Boot支持通过配置进行分布式的session存储

**spring.session.store-type = redis**

[spring session]: https://docs.spring.io/spring-session/docs/current/reference/html5/index.html#httpsession-why	"官方文档"
[spring-session-data-redis]: https://spring.io/projects/spring-session-data-redis#overview	"官方文档"



#### redis中存储的sessionId和返回的cookie不同？

​	***浏览器中的cookie:***

```shell
Cookie: JSESSIONID=45FEC498205666FE7DAB1F1658837ADE
```

​	***redis中：***

```shell
redis-cli del spring:session:sessions:7e8383a4-082c-4ffe-a4bc-c40fd3363c5e
```



* redis中session的存储情况

>##### Saving a Session
>
>Each session is stored in Redis as a `Hash`. Each session is set and updated by using the `HMSET` command. The following example shows how each session is stored:



* 通过`CookieSerializer`来生成对应的浏览器cookie，默认的实现类为`DefaultCookieSerializer.class`

>### 9.13. Using `CookieSerializer`
>
>A `CookieSerializer` is responsible for defining how the session cookie is written. Spring Session comes with a default implementation using `DefaultCookieSerializer`.



```java
	@Bean
	public CookieSerializer cookieSerializer() {
		DefaultCookieSerializer serializer = new DefaultCookieSerializer();
		serializer.setCookieName("JSESSIONID"); 
		serializer.setCookiePath("/"); 
		serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$"); 
		return serializer;
	}
```



通过debug模式检查到进行序列化的***session***信息是redis存储的session信息

```txt
cookieValue = {CookieSerializer$CookieValue@20946} 
 request = {SessionRepositoryFilter$SessionRepositoryRequestWrapper@20948} 
 response = {HttpServletResponseImpl@20991} 
 cookieValue = "0374bfac-0811-4308-884f-c08c59bf975c"
 cookieMaxAge = -1
this.jvmRoute = null
```



之后再进行默认的Base64的序列化加密操作，返回给前台 `SESSIONID=#{context}`



▶️***个人总结：***

1. Spring通过拦截器，替代了原本的Container中的session处理过程；
2. 当请求过来时，Spring通过可持久化的存储方式，来实现了分布式应用环境下的session一致性问题；
3. redis存储session的形式为 `HMSET` 方式；
4. 返回给浏览器的cookie时，通过 `DefaultCookieSerializer` 对存储在redis中的name值进行了base64转化；
5. redis中通过三个key-value值，来保证session过期后的失效操作。



[官方文档 - spring session]: https://docs.spring.io/spring-session/docs/2.4.3/reference/html5/#introduction	"Spring Session"

