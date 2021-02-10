#坑

 

# Mybatis实战

### 模糊查询

`'%#{name}%' `使用会报错，导致无法使用模糊查询

解释：MyBatis转换后(‘%#{name}%')会变为(‘%?%'),而(‘%?%')会被看作是一个字符串，所以Java代码在执行找不到用于匹配参数的 ‘?' ,然后就报错了

`"%"#{name}"%"`使用报错，oracle错误，无法识别%标示符；

`"%"||#{name}||"%"`  ，`"''%"||#{name}||"%'"`使用报错，ORA-00904，无法识别`"%"`标示符；

解释：Mybatis解析为以下语句，然后执行失败

```sql
select * from student where name like "%"||'张三'||"%"
```



1. `CONCAT(CONCAT('%',#{name},'%')` 使用oracle拼接字符串方式

2. `INSTR(name,#{name})` 使用oracle字符串查询功能

3. `<bind name = "bindName" value = "'%' + _parameter.name + '%'"/>` 使用bind的标签操作

4. 使用`‘%${name}%’ ` 拼接字符串即可

5. `'%'||#{name}||'%'` 三个字符串拼接，mybatis再转化为整个字符串

   ```sql
   select * from student where name like '%'||'张三'||'%'
   ```





### association相关逻辑

`association`可以用于取代oracle中的`left join`语法，通过两次相关查询的方式

<p style = 'color:red'>ps : association不能向result一样使用；</p>

示例代码：/Users/swk/光大银行工作内容/BAK/20201105/mybatis code

参考：

https://blog.csdn.net/zhiguoliu11/article/details/82995444

https://www.jianshu.com/p/018c0f083501





### if - else

```xml
<select id="selectSelective" resultMap="xxx" parameterType="xxx">
    select
    <include refid="Base_Column_List"/>
    from xxx
    where del_flag=0
    <choose>
        <when test="xxx !=null and xxx != ''">
            and xxx like concat(concat('%', #{xxx}), '%')
        </when>
        <otherwise>
            and xxx like '**%'
        </otherwise>
    </choose>
</select>
```



mybatis中的if-else

```xml
<choose>
    <when test="">
        //...
    </when>
    <otherwise>
        //...
    </otherwise>
</choose>
```





##Mybatis Generator Plugin 实战使用

mvn集成的基于mybatis-generator的使用



1、增加maven依赖

```xml
<plugin>
  <groupId>org.mybatis.generator</groupId>
  <artifactId>mybatis-generator-maven-plugin</artifactId>
  <version>1.3.5</version>
  <configuration>
    <!-- 在控制台打印执行日志 -->
    <verbose>true</verbose>
    <!-- 重复生成时会覆盖之前的文件-->
    <overwrite>true</overwrite>
    <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
  </configuration>
  <!-- 数据库连接选择8.0以上的，因为用的mysql8.0-->
  <dependencies>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.16</version>
    </dependency>
  </dependencies>
</plugin>
```



2、增加generatorConfig配置文件

 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!-- context 是逆向工程的主要配置信息 -->
    <!-- id：起个名字 -->
    <!-- targetRuntime：设置生成的文件适用于那个 mybatis 版本 -->
    <context id="default" targetRuntime="MyBatis3">
        <!--optional,指在创建class时，对注释进行控制-->
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!--jdbc的数据库连接 wg_insert 为数据库名字-->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/demo?useUnicode=true&amp;characeterEncoding=utf-8&amp;serverTimezone=UTC"
                        userId="root"
                        password="12345678"/>
        <!--非必须，类型处理器，在数据库类型和java类型之间的转换控制-->
        <javaTypeResolver>
            <!-- 默认情况下数据库中的 decimal，bigInt 在 Java 对应是 sql 下的 BigDecimal 类 -->
            <!-- 不是 double 和 long 类型 -->
            <!-- 使用常用的基本类型代替 sql 包下的引用类型 -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>
        <!-- targetPackage：生成的实体类所在的包 -->
        <!-- targetProject：生成的实体类所在的硬盘位置 -->
        <javaModelGenerator targetPackage="com.seed.mybatisRedis.entity"
                            targetProject="src/main/java">
            <!-- 是否允许子包 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 是否对modal添加构造函数 -->
            <property name="constructorBased" value="true"/>
            <!-- 是否清理从数据库中查询出的字符串左右两边的空白字符 -->
            <property name="trimStrings" value="true"/>
            <!-- 建立modal对象是否不可改变 即生成的modal对象不会有setter方法，只有构造方法 -->
            <property name="immutable" value="false"/>
        </javaModelGenerator>
        <!-- targetPackage 和 targetProject：生成的 mapper 文件的包和位置 -->
        <sqlMapGenerator targetPackage="mapper"
                         targetProject="src/main/resources">
            <!-- 针对数据库的一个配置，是否把 schema 作为字包名 -->
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!-- targetPackage 和 targetProject：生成的 interface 文件的包和位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.seed.mybatisRedis.entity.dao" targetProject="src/main/java">
            <!-- 针对 oracle 数据库的一个配置，是否把 schema 作为字包名 -->
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>
        <!-- tableName是数据库中的表名，domainObjectName是生成的JAVA模型名，后面的参数不用改，要生成更多的表就在下面继续加table标签 -->
        <table tableName="demo" domainObjectName="Demo"
               enableCountByExample="true" enableUpdateByExample="true"
               enableDeleteByExample="true" enableSelectByExample="true"
               selectByExampleQueryId="true"/>
    </context>
</generatorConfiguration>
 ```



3、执行maven插件操作，生成对应的功能具体类

生成的代码参考：





## XML的转义字符



当遇到大于或者小于号时，mapper文件会认为是特殊字符，故需要进行转移

| `&lt;`   | <    | 小于号 |
| -------- | ---- | ------ |
| `&gt;`   | >    | 大于号 |
| `&amp;`  | &    | 和     |
| `&apos;` | ’    | 单引号 |
| `&quot;` | "    | 双引号 |

