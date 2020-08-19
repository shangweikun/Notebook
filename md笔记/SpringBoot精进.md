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

<img src="/Users/swk/ScreenPictures/截屏2020-08-16 下午10.08.37.png" alt="截屏2020-08-16 下午10.08.37" style="zoom: 50%;" />



* RelaxedBinding

  默认SpingBoot支持一下的命名规则

  ![截屏2020-08-16 下午10.10.34](/Users/swk/ScreenPictures/截屏2020-08-16 下午10.10.34.png)

## 配置加载的抽象

* PropertySource

  自己定义一个配置加载内容

  特别适合于：配置中心与Spring Boot相结合

  ***demo：property-source-demo***

  