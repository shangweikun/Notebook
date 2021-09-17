# Spring实战学习：

参考：https://blog.csdn.net/weixin_37910453/article/details/90081929	



###Spring项目console输出带颜色

配置VM参数即可

```shell
-Dspring.output.ansi.enabled=ALWAYS
```





### Spring启动原理补充

参考资料：

https://www.jianshu.com/p/7248b3ff5ca7





### @RestController 和 Controller 的区别？

@RestController is a stereotype annotation that combines @ResponseBody and @Controller.



\1. The @Controller is a common annotation that is used to mark a class as Spring MVC Controller while @RestController is a special controller used in [RESTFul web services](http://javarevisited.blogspot.sg/2015/08/difference-between-soap-and-restfull-webservice-java.html) and the equivalent of @Controller + @ResponseBody.

\2. The **@RestController** is relatively new, added only on Spring 4.0 but @Controller is an old annotation, exists since Spring started supporting annotation, officially it was added on Spring 2.5 version. You can learn more about @RestController and other Spring 4 annotations on **[Master RESTful Web Services with Spring Boot](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspring-web-services-tutorial%2F)** course on Udemy. Spring boot really makes it easy to develop REST APIs with spring.

\3. The @Controller annotation indicates that the class is a "Controller" like a web controller while @RestController annotation indicates that the class is a controller where @RequestMapping methods assume **@ResponseBody** semantics by default i.e. servicing REST API.

Read more: https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html#ixzz6nmAZfP9q



鸣谢：https://www.jianshu.com/p/c89a3550588a







# Bean 加载依赖

**【条件注解】**

| 条件化注解                      | 配置生效条件                   |
| ------------------------------- | ------------------------------ |
| @ConditionalOnClass             | Classpath里有指定的类          |
| @ConditionalOnMissingClass      | Classpath里缺少指定的类        |
| @ConditionalOnBean              | 配置了某个特定Bean             |
| @ConditionalOnMissingBean       | 没有配置特定的Bean             |
| @ConditionalOnProperty          | 指定的配置属性要有一个明确的值 |
| @ConditionalOnResource          | Classpath里有指定的资源        |
| @ConditionalOnWebApplication    | 这是一个Web应用程序            |
| @ConditionalOnNotWebApplication | 这不是一个Web应用程序          |
| @ConditionalOnExpression        | 给定的SpEL表达式计算结果为true |

​	

以下实例中，通过互斥的 **条件注解** @*ConditionalOnMissingClass* 和 @*ConditionalOnClass* 实现对于***Formatter***的注入

```java
@Configuration
public class FormatterAutoConfiguration {

    @Bean
    @ConditionalOnMissingClass(value = "com.fasterxml.jackson.databind.ObjectMapper")
    public Formatter defaultFormatter(){
        return new DefaultFormatter();
    }

    /**
     * json格式输出
     * @return
     */
    @Bean
    @ConditionalOnClass(name = "com.fasterxml.jackson.databind.ObjectMapper")
    public Formatter jsonFormatter(){
        return new JsonFormatter();
    }
}
```



[官方链接]: https://docs.spring.io/spring-boot/docs/2.1.6.RELEASE/reference/htmlsingle/#boot-features-condition-annotations	"49.3 Condition Annotations"

