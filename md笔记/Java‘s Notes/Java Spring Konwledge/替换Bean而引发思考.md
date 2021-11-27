### POIN中替换某个bean的方法

#### 初步

通过replace 重新注册一个新的bean；

使用中发现：当在 `main` 方法中进行替换操作时，已经通过自动注入的bean的引用，无法进行修改；

如果重新通过 `getBean` 方式，则会生成一个全新的实例对象，并将新的bean替换进去；

```java
	@Override
	public Object getBean(String name) throws BeansException {
		return doGetBean(name, null, null, false);
	}
```



```java
	@SuppressWarnings("unchecked")
	protected <T> T doGetBean(
			String name, @Nullable Class<T> requiredType, @Nullable Object[] args, boolean typeCheckOnly)
			throws BeansException {
        
        //...
        
				RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);
				checkMergedBeanDefinition(mbd, beanName, args);
        //...
    }
```



#### 进一步

考虑使用 `PostProcesser` 来进行拦截注入，替换掉 `目标bean`;



以上方法是通过生成一个 PersonService的子类并组合原本的Spring生成的bean

-------------------------------------------------------------------------------------------------------------------------



#### 再进一步

使用 `CGLIB动态代理`，代理bean，拦截对应的方法；



***PS：***`JDK动态代理` 失败;

```shell
The bean 'A' could not be injected as a 'xxx.BClassName' because it is a JDK dynamic proxy that implements:
   CInterface
```

试图通过代理一个 `IPersonService` 接口，生成的代理类 `Proxy` 注入到 `PersonService.class`，这个行为是错误的。



该步骤中，考虑动态生成一个bean，通过反射来拦截指定的方法

-----------



#### 最终

使用 `@Aspect`  中环绕增强对应的find方法 `@Around` 





### 年轻人的第一个starter



1. `@ConditionalOnClass`，当classpath下发现该类的情况下进行自动配置。
    2. `@ConditionalOnMissingBean`，当Spring Context中不存在该Bean时。
        3. `@ConditionalOnProperty(prefix = "example.service",value = "enabled",havingValue = "true")`，当配置文件中example.service.enabled=true时。



```xml
<groupId>com.example</groupId>
<artifactId>spring-boot-younger-starter</artifactId>
<version>1.0.0-SNAPSHOT</version>
```



鸣谢：

https://www.xncoding.com/2017/07/22/spring/sb-starter.html


