## java命令工具

### jmod

鸣谢：

https://docs.oracle.com/javase/9/tools/jmod.htm#JSWOR-GUID-0A0BDFF6-BE34-461B-86EF-AAC9A555E2AE

https://juejin.cn/post/6844903501311524871



## JavaEE安装

1、官方网站下载

***官方网站：***

https://www.oracle.com/java/technologies/java-ee-7-sdk-with-jdk-u45-downloads.html

***glassfish下载：***

https://www.oracle.com/java/technologies/oracle-java-archive-downloads.html

2、执行shell命令

```shell
DISPLAY=:0 ./java_ee_sdk-7-jdk7-macosx-x64.sh 
```

鸣谢：

https://stackoverflow.com/questions/20607777/what-display-settings-needed-for-running-sh-installer-for-java-7-ee-sdk-on-mac



FAILURE 执行失败，一直在等待安装



## JAVA 修改jdk

增加jar包

```shell
jar -cvf xxx.jar -C 路径
#意思是将路径下的文件压缩到 jar 中
jar -xvf xxx.jar 
#解压jar包
```

解压jar包

```shell
unzip xxx.jar -d 路径
```





### 打印JDK或者CGLIB动态代理的class

​		System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY, "D:\\class");  --该设置用于输出cglib动态代理产生的类

​		System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");   --该设置用于输出jdk动态代理产生的类或者在启动配置VM Options中设置 -Dsun.misc.ProxyGenerator.saveGeneratedFiles=true



### TypeReference

- Jackson ObjectMapper 提供了TypeReference支持对泛型对象的反序列化；
- 对于获取泛型类型信息的场景，TypeReference是一个可以参考的通用解决方案。



TypeReference主要源码：

```java
protected TypeReference()
    {
        Type superClass = getClass().getGenericSuperclass();
        _type = ((ParameterizedType) superClass).getActualTypeArguments()[0];
    }
```

***使用实例：***

```java
List<UserResource> list = new ObjectMapper().readValue(userResourcesStr, new TypeReference<List<UserResource>>(){});
```

