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

