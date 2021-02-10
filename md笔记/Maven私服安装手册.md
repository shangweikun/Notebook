# Maven私服安装



##1、下载专业软件

http://www.sonatype.org/nexus/

* NEXUS OSS [OSS = Open Source Software，开源软件——免费]

* NEXUS PROFESSIONAL -FREE TRIAL [专业版本——收费]。





```xml
<dependency>
			<groupId>commons-dbcp</groupId>
			<artifactId>commons-dbcp</artifactId>
			<version>1.4</version>
			<scope>runtime</scope>
</dependency>
```

runtime表示在构建编译阶段不需要，只在test和runtime需要。这种主要是指代码里并没有直接引用而是根据配置在运行时动态加载并实例化的情况。虽然用runtime的地方改成compile也不会出大问题，但是runtime的好处是可以避免在程序里意外地直接引用到原本应该动态加载的包。例如JDBC连接池

