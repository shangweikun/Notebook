

# Spring Boot学习

https://github.com/macrozheng/mall-admin-web.git



@RestController

Spring的web拦截器



@RequestMapping

web地址的映射





### 数据库相关

可以用于Spring Boot启动时初始化数据库 : data.sql 、schema.sql

**自动装配spring.datadource.***

**配置多个数据源 **

1. **排除自动装配的spring boot类**

2. **指定不同的bar.datasource等的配置**
3. **初始化对应的DataSource**



原生spring

DataSource的初始化：

1. 代码生成；
2. xml配置bean。



···HikariCP



ps: 数据库相关bean建议加入@Repository

ps: SpringMVC 使用 @Controller



···lombok



***JdbcTemplate***

jdbc的封装操作，内部有batch方式操作sql语句；



#### 编程式事务

​			***TransactionTemplate*** 



#### 声明式事务

​			***@Transactional***

**同一类内方法调用，无论被调用的b()方法是否配置了事务，此事务在被调用时都将不生效。**

🔗：https://blog.csdn.net/blacktal/article/details/79345902



Spring自定义数据库异常调整***sql-error-codes.xml***



SpringTest测试情况

@Test(exception = [target].class)



常见注解

@Configuration 配置类标示；

@ImportResource 注入另外的配置文件；

@ComponentScan 是插件扫描；

@Bean 表明是是个bean，可以修饰方法，**方法的return结果可以加入bean管理**；

@ConfigurationProperties HikariCP的一些配置；

@Repository 数据库访问层的bean

@Service 服务层的注解



@Controller/@RestController拦截器

@Autowired 自动装配对应的bean

@Qualifier/@Resource 可以通过bean名称，指定装配的bean

@Value bean里面注入一些名字



***endpoint***			 用于Actuator等的监控  可单独配置health，beans



数据库中间件



事务传播级别中

Nested 子事务 

REQUEST_NEW 自治事务

拓展：***数据库的触发器*** ， ***腾龙的事务处理*** 以及 ***数据库自治事务***



Druib

removeAbandoned不开启



扩展机制：

FilterChain

责任链，一步一步调用获取filter

ps：逐步加强Filter ***Druid很多通过责任链扩展***



---------------

##Spring Data JPA（Java Persistence API）

###Hibernate

Object - Relational Mapping



Spring.data的子模块中

```
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.format_sql=true
```



### 定义

通过注解来实现

**实体**

* @Entity

* @MappedSuperclass

* 现在@Table(name)



**主键**

* @Id

  @GeneratedValue

  @SequenceGenerator



**映射**

@Column



**关系**



#SpringBoot咖啡店

Repository

@EnableJpaRepositories



```html
<html>
  <body>
    <p>
      Field userDemoRepository in com.example.springtestjpa.SpringTestJpaApplication required a bean named 'entityManagerFactory' that could not be found.
    </p>
    <br />
    <p>
      <strong>Hibernate的jar包导入失败，导致无法装配成功</strong>
  </body>
</html>
```



### JPA Repository源码分析

JpaRepositoriesRegistrar

RepositoryBeanDefinitionRegistrarSupport.***registerBeanDefinitions***



RepositoryFactorySupport.***getRepository***

中增加Advice

* QueryExecutorMethodInterceptor

  ```java
  private Object doInvoke(MethodInvocation invocation) throws Throwable {
  
     Method method = invocation.getMethod();
  
     if (hasQueryFor(method)) {
  
        QueryMethodInvoker invocationMetadata = invocationMetadataCache.get(method);
  
        if (invocationMetadata == null) {
           invocationMetadata = new QueryMethodInvoker(method);
           invocationMetadataCache.put(method, invocationMetadata);
        }
  
        RepositoryQuery repositoryQuery = queries.get(method);
        return invocationMetadata.invoke(repositoryQuery, invocation.getArguments());
     }
  
     return invocation.proceed();
  }
  ```

Part语法解析



### Mybatis框架

半持久性框架

```
mybatis.type-handlers-
package=geektime.spring.data.mybatisdemo.handler
mybatis.configuration.map-underscore-to-camel-case=true
```

@MapperScan

@Mapper 定义接口

@XML与注解



#### Generator

http://mybatis.org/generator/

自动生成generator的代码



----





# No-sql

###MongoDB

docker容器操作

docker exec 

```shell
Mongo -u admin -p admin
show dbs
use springbucks
show user
```







###Reids

客户端：Jedis，Lettuce（支持读写分离操作）

Redis 的哨兵与集群部署



RedisTemplate操作

StringTemplate 			K，V都是String默认

***一定要设置超时时间***

***一定要设置超时时间***

***一定要设置超时时间***

```java
@ReadingConverter   //从redis读到cache
@WritingConverter		//写入redis
```



#### Redis Repository

@EnableRedisRepositories



###Spring缓存支持

```java
@EnableCaching(proxyTargetClass = true) //使用缓存
@Cacheable //开启缓存
@CacheEvict //缓存清理
```



---





# Reactor

http://projectreactor.mydoc.io/?t=44478

https://projectreactor.io/docs/core/release/reference/

* Operators - Publisher/Subscriber

* Backpressure
  * onReuquest ...
* Schedulers elastic - 空闲6秒被回收
* ErrorDeal





### Spring Data Redis

####Reactive Redis

maven依赖：

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
   <groupId>com.h2database</groupId>
   <artifactId>h2</artifactId>
   <scope>runtime</scope>
</dependency>
<dependency>
   <groupId>org.projectlombok</groupId>
   <artifactId>lombok</artifactId>
   <optional>true</optional>
</dependency>
```



####Reactive MongoDB

并发执行中的部分区别

```java
startFromInsertion(() -> {
   log.info("Runnable");
   decreaseHighPrice();
});
```





###Spring Data R2DBC

👌操作传统***关系型数据库***

* ConnectionFactory

* DatabaseClient

  * execute().sql(SQL)

  * select()/insert()

    

    ***Repository*** R2DBC的

    

milestone的仓库配置

```xml
<repositories>
   <repository>
      <id>springsource-milestone</id>
      <url>http://repository.springsource.com/maven/bundles/milestone</url>
   </repository>
   <repository>
      <id>atlassian-m2-repository</id>
      <url>https://m2proxy.atlassian.com/repository/public</url>
   </repository>
</repositories>
```



###AOP打印数据访问层摘要

***AOP***：

* Advice: ***MethodInterceptor***
* ***TransactionInterceptor.invoke***

在其中独立切面插入了一个事务



注解：

* @EnableAspectJAutoProxy
* @Aspect
* @Pointcut(execution(public * *(..)))



打印Sql

* P6Spy http://github.com/p6spu/p6spy

* https://github.com/alibaba/druid/siki/Druid

  使用log4j2日志输出



**ps:Spring带有完整CGLIB**





# Spring MVC实践

Spring MVC 

 

* Controller
* xxxResolver
* HandlerMapping



* @Controller
  * @RestController
* @RequestMapping
* @RequestBody/ @ResponseBody/ 

PostMan





Web Context

Servlet Context / Aop拦截 

* 疑问：不同的ApplicationContext家在相同名称的bean，为什么是不同的呢？

* 当Aspect的注入到Context时，对所有的Context中的bean生效

  当Context2 加入 Context1 中时，Context2中的Aspect对Context1中的bean生效

  ```java
  ClassPathXmlApplicationContext barContext = new ClassPathXmlApplicationContext(
          new String[]{"applicationContext.xml"},fooContext);
  ```



###定义Controller

Spring MVC文档 ：请求处理流程

   

* Front controller
* Controller
* View template



MultipartHttpServletRequest



Controller

demo：***complex-controller-demo***

```java
@GetMapping
@PostMapping(path = "/", consumes = MediaType.APPLICATION_JSON_VALUE,
            produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
```

Context-Type - ***consumes***

Accept - ***produces***

https://www.runoob.com/http/http-content-type.html

`Accept`：发送端（客户端）希望接受的数据类型。

`Content-Type`：发送端（客户端|服务器）发送的实体数据的数据类型。

​	

code - **more-complex-controller-demo**

```java
@Valid 
@PostMapping(path = "/", consumes = MediaType.APPLICATION_FORM_URLENCODED_VALUE)
    @ResponseBody
    @ResponseStatus(HttpStatus.CREATED)
```

不同的consumes，可以使用相同的***path｜value***



* 疑问🤔️***Formatter***

NewCoffeeRequest为什么需要这个类



### MVC View解析

ViewResolver 与 View 接口

DispatcherServlet

* initStragegies（）
* doDispatch（）



@ResponseBody

handle（）中

```java
this.returnValueHandlers
```



目标方法执行

***InvocableHandlerMethod***

```java
return getBridgedMethod().invoke(getBean(), args);
```

***ServletInvocableHandlerMethod***

```java
public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer,
			Object... providedArgs) throws Exception {

		Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
		setResponseStatus(webRequest);

		if (returnValue == null) {
			if (isRequestNotModified(webRequest) || getResponseStatus() != null || mavContainer.isRequestHandled()) {
				mavContainer.setRequestHandled(true);
				return;
			}
		}
		else if (StringUtils.hasText(getResponseStatusReason())) {
			mavContainer.setRequestHandled(true);
			return;
		}

		mavContainer.setRequestHandled(false);
		Assert.state(this.returnValueHandlers != null, "No return value handlers");
		try {
			this.returnValueHandlers.handleReturnValue(
					returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
		}
		catch (Exception ex) {
			if (logger.isTraceEnabled()) {
				logger.trace(formatErrorForReturnValue(returnValue), ex);
			}
			throw ex;
		}
	}

```



重定向：

* redirect

* forward



### MVC 常用的视图格式

**WebMvcAutoConfigurationebMvc**

* **HttpMessageConveters**	变量：messageConvertersProvider

* **ViewResolver**



####	Jackson-based JSON / XML

***MessageConverter***

demo：**json-view-demo**

支持xml和json的返回报文



	#### 	Thymeleaf

相关默认配置

* spring.thymeleaf.prefix=classpath:/templates/

Demo:

有一个关于   ***重定向***   的代码逻辑





### 静态资源和缓存

为什么java中不建议使用静态资源和缓存呢？

CDN - 》 静态资源

GateWay - 》 缓存（增加一层，减少对应用的访问）



core：***WebMvcConfigurer.addResourceHandlers()***

可以配置路径

* spring.mvc.static-path-pattern=/**
* Spring.resource.static-locations=classpath:/META-INF/resources/,class path:/resources

### 异常处理

@ExceotionHandler

添加位置：@Controller/@RestController

​					@ControllerAdvice / RestControllerAdvice

***HandlerExceotionResolver***接口

demo：**exception-demo**



###AOP切入点

***HandlerInterceptor***接口

```java
StopWatch //时间记录类
```



WebMvcConfigurer.addInterceptors()增加拦截的切入点





----

# PS



## Money

```xml
<dependency>
   <groupId>org.joda</groupId>
   <artifactId>joda-money</artifactId>
   <version>1.0.1</version>
</dependency>
```



