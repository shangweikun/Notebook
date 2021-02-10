

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

