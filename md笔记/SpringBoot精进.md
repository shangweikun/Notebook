# SpringBoot精进



## 背后的秘密  

<p style="color:red">Spring Boot到底是什么？</p>

<p style="color:red">Undertow?</p>



## 四大核心

* 自动配置
* Starter Dependency
* Spring Boot CLI
* Acturator



## 自动配置的秘密㊙️

@EnableAutoConfiguration

如果存在目标的Bean，则自动配置可能会失效 ***\*@ConditionOnMissingBean\****实现



观察自动配置的判断结果

* --debug



自己写一个自动配置：
1、@Configuration

2、@Conditional - 一系列

3、定位自动配置的



ps：加入已有的module模块 - Ideal的操作

***\*demo\****：autoconfiguration-demo



FailureAnalysis - 失败分析器



###Bean的LifeCycle

下面是基于Bean的生命周期的一些配置：初始化和销毁时的操作

<img src="/Users/swk/Desktop/截屏2020-08-10 下午10.38.22.png" alt="截屏2020-08-10 下午10.38.22" style="zoom:25%;" />





***\*BeanFactoryPostProcesser\****

```java
@SpringBootApplication
```

该注解可以扫描当前包名，及以下包名的所有Bean





## 起步依赖

* 可通过覆盖同名版本号 - 用于修改Spring Boot中默认的依赖项目version

Spring Boot通过Starter可以简化对应的依赖，更加方便开发者工作



* 配置加载的优先序：

{profile}名称在前，jar包外部依赖优先

加载路径位置：

<img src="/Users/swk/ScreenPictures/截屏2020-08-16 下午10.08.37.png" alt="截屏2020-08-16 下午10.08.37" style="zoom: 25%;" />



* RelaxedBinding

  默认SpingBoot支持一下的命名规则

  <img src="/Users/swk/ScreenPictures/截屏2020-08-16 下午10.10.34.png" alt="截屏2020-08-16 下午10.10.34" style="zoom:25%;" />

## 配置加载的抽象

* PropertySource

  自己定义一个配置加载内容

  特别适合于：配置中心与Spring Boot相结合

  ***demo：property-source-demo***





##Actuator Endpoint

* 访问方式：JMX｜Http

通过jconsole｜VisualVM进行监控



* Actuator配置参数：

<img src="/Users/swk/ScreenPictures/截屏2020-08-31 上午7.33.08.png" alt="截屏2020-08-31 上午7.33.08" style="zoom:25%;" />



## 定制Health Indicator

 

<img src="/Users/swk/ScreenPictures/截屏2020-08-31 下午12.29.30.png" alt="截屏2020-08-31 下午12.29.30" style="zoom:25%;" />

* 自定义Health Indicator

  * 实现HealthIndicator接口
  * 根据自定义检查返回状态

  ```java
  @Override
      public Health health() {
          long count = coffeeService.getCoffeeCount();
          Health health;
          if (count > 0) {
              health = Health.up()
                      .withDetail("count", count)
                      .withDetail("message", "We have enough coffee.")
                      .build();
          } else {
              health = Health.down()
                      .withDetail("count", 0)
                      .withDetail("message", "We are out of coffee.")
                      .build();
          }
          return health;
      }
  ```

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img src="/Users/swk/ScreenPictures/截屏2020-08-31 下午1.27.33.png" alt="截屏2020-08-31 下午1.27.33" style="zoom: 25%;" />



## Spring Boot Admin

* 三方的管理界面
* server端用与监控客户端的运行情况

<img src="/Users/swk/ScreenPictures/截屏2020-08-31 下午9.17.41.png" alt="截屏2020-08-31 下午9.17.41" style="zoom:25%;" />



* client端需要在`服务端`进行注册，绑定url和登录用的username｜password（add）

  #												instance具体是什么含义呢？

<img src="/Users/swk/ScreenPictures/截屏2020-08-31 下午9.18.22.png" alt="截屏2020-08-31 下午9.18.22" style="zoom:25%;" />



##HTTPS

![截屏2020-08-31 下午9.45.29](/Users/swk/ScreenPictures/截屏2020-08-31 下午9.45.29.png)

![截屏2020-08-31 下午9.46.04](/Users/swk/ScreenPictures/截屏2020-08-31 下午9.46.04.png)

![截屏2020-08-31 下午9.47.05](/Users/swk/ScreenPictures/截屏2020-08-31 下午9.47.05.png)

![z](/Users/swk/ScreenPictures/截屏2020-08-31 下午9.49.43.png)

##HTTP2

![截屏2020-08-31 下午9.59.11](/Users/swk/ScreenPictures/截屏2020-08-31 下午9.59.11.png)

![截屏2020-08-31 下午10.01.09](/Users/swk/ScreenPictures/截屏2020-08-31 下午10.01.09.png)

![截屏2020-08-31 下午10.31.18](/Users/swk/ScreenPictures/截屏2020-08-31 下午10.31.18.png)

# 可执行jar包

## 本质：

**\*.\xxx.jar\***可执行的原理是因为，springboot在其生成的头部加入了一段shell，根据shell从前往后执行的特性，可以直接运行**jar**包



* 默认的参数配置，可在运行时指定

<img src="/Users/swk/ScreenPictures/200902/截屏2020-09-02 上午8.01.24.png" alt="截屏2020-09-02 上午8.01.24" style="zoom:25%;" />



* springboot封装的maven打包以来
  * **\<executable>** 指定为 **true**

<img src="/Users/swk/ScreenPictures/200902/截屏2020-09-02 上午8.02.20.png" alt="截屏2020-09-02 上午8.02.20" style="zoom:25%;" />

* 展示MANIFEST.MF信息

<img src="/Users/swk/ScreenPictures/200902/截屏2020-09-02 上午8.04.25.png" alt="截屏2020-09-02 上午8.04.25" style="zoom:25%;" />



# Docker镜像构建

* 什么是docker镜像

<img src="/Users/swk/ScreenPictures/200902/截屏2020-09-02 下午10.19.07.png" alt="截屏2020-09-02 下午10.19.07" style="zoom:25%;" />



# SpringBoot启动原理

重要的事件回调机制

**ApplicationContextInitializer**

**SpringApplicationRunListener**



只需要放在ioc容器中

**ApplicationRunner**

**CommandLineRunner**



## 1、初始化SpringApplication对象

* 传入**SpringApplication**类，对其进行初始化，然后执行run方法

```java
/**
	 * Create a new {@link SpringApplication} instance. The application context will load
	 * beans from the specified primary sources (see {@link SpringApplication class-level}
	 * documentation for details. The instance can be customized before calling
	 * {@link #run(String...)}.
	 * @param resourceLoader the resource loader to use
	 * @param primarySources the primary bean sources
	 * @see #run(Class, String[])
	 * @see #setSources(Set)
	 */
	@SuppressWarnings({ "unchecked", "rawtypes" })
	public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
    //设置Initializers
		setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    //设置Listeners
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		this.mainApplicationClass = deduceMainApplicationClass();
	}
```



```java
static WebApplicationType deduceFromClasspath() {
  //判断app是否为web应用【Reactive版本应用为何要做特殊判断呢？】
		if (ClassUtils.isPresent(WEBFLUX_INDICATOR_CLASS, null) && !ClassUtils.isPresent(WEBMVC_INDICATOR_CLASS, null)
				&& !ClassUtils.isPresent(JERSEY_INDICATOR_CLASS, null)) {
			return WebApplicationType.REACTIVE;
		}
		for (String className : SERVLET_INDICATOR_CLASSES) {
			if (!ClassUtils.isPresent(className, null)) {
				return WebApplicationType.NONE;
			}
		}
		return WebApplicationType.SERVLET;
	}
```



##2、执行run方法

```java
/**
	 * Run the Spring application, creating and refreshing a new
	 * {@link ApplicationContext}.
	 * @param args the application arguments (usually passed from a Java main method)
	 * @return a running {@link ApplicationContext}
	 */
	public ConfigurableApplicationContext run(String... args) {
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();
		ConfigurableApplicationContext context = null;
		Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
		configureHeadlessProperty();
    
    //获取SpringApplicationRunListeners，从META-INF/spring.factories类下面加载对应的Listeners
		SpringApplicationRunListeners listeners = getRunListeners(args);
    //启动Listeners
		listeners.starting();
    
    
		try {
      //封装命令行参数
			ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
      //准备环境，同时为Lisners也执行环境准备method【内部的bindToSpringApplication方法设计非常漂亮】
			ConfigurableEnvironment environment = prepareEnvironment(listeners, applicationArguments);
      
      //排除已经ignore的Bean
			configureIgnoreBeanInfo(environment);
      
      //打印Banner
			Banner printedBanner = printBanner(environment);
      
      //创建ApplicationContext;
      //决定是普通的ioc，还是web的ioc
			context = createApplicationContext();
			context.setApplicationStartup(this.applicationStartup);
      
			exceptionReporters = getSpringFactoriesInstances(SpringBootExceptionReporter.class,
					new Class<?>[] { ConfigurableApplicationContext.class }, context);
      //准备上下文环境；将environment保存在ioc中；
      //applyInitializers(context);回调所有Initializers的initialize(context);
      //同时执行Listeners的环境回调操作：contextPrepared(context);｜contextLoaded(context);
			prepareContext(context, environment, listeners, applicationArguments, printedBanner);
      
      //单线程操作：refresh()方法
      //refresh()： 初始化ioc，并刷新BeanFactory，beanPostProcess进行操作；
      //					  注册监听器Listeners
      //						注册，配置，并加载所有的组件
			refreshContext(context);
      
  		//空方法
			afterRefresh(context, applicationArguments);
			stopWatch.stop();
			if (this.logStartupInfo) {
				new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
			}
      
      //Listeners启动
			listeners.started(context);
      //执行所有Runners
      //ApplicationRunner.class｜CommandLineRunner.class
			callRunners(context, applicationArguments);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, exceptionReporters, listeners);
			throw new IllegalStateException(ex);
		}

		try {
      //监听器启动并运行，同时可以通过ioc容器进行特殊Bean的操作
			listeners.running(context);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, exceptionReporters, null);
			throw new IllegalStateException(ex);
		}
		return context;
	}
```

