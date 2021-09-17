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



### instr 和 like

`instr(‘源字符串’,’查询字符串’)>0` 相当于 `‘源字符串’ like ’%查询字符串%’ =true`

`instr(‘源字符串’,’查询字符串’)=0` 相当于 `‘源字符串’ like ’%查询字符串%’ =false`

～ 1.%a%方式：
select * from pub_yh_bm t where instr(t.chr_bmdm,'2')>0
等份于：
select * from pub_yh_bm t where t.chr_bmdm like '%2%'

～ 2.%a方式：
select * from pub_yh_bm t
where instr(t.chr_bmdm,'110101')=length(t.chr_bmdm)-length('110101')+1
等份于：
select * from pub_yh_bm t where t.chr_bmdm like '%110101'

～ 3.a%方式：
select * from pub_yh_bm t where instr(t.chr_bmdm,'11010101')=1
等份于：
select * from pub_yh_bm t where t.chr_bmdm like '11010101%'



`like`有时可以用到索引，例如：`name like '李%'`

而当下面的情况时索引会失效：`name like '%李'`

 

***\*简单测试来看，instr\**的效率是比like要高些（orace对内建函数做了优化），而且使用like时，一些索引是不能用的，但oracle支持函数索引，如果使用函数索引的话，执行更快。**

***\*一般的数据库中，instr\**和like的效率是没有多大差别的，但对于oracle数据库可以通过函数索引来提高instr的执行效率。**





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



### Mybatis 中当传入一个Map，怎么接受一个List呢？

demo如下：

```java
{
    List<String> list;
    map.put("userList",list);
}
```

```xml
<select id="selectByMap1" parameterType="java.util.Map" resultType="com.example.demo.dto.User">
    SELECT id,name,note from USER
    where id in
    <foreach collection="userMap" separator="," open="(" close=")" item="id">
        #{id}
    </foreach>
</select>
```



https://www.cnblogs.com/yl97/p/13380565.html





### Mybatis 的批量插入踩坑

环境：mybatis，linux，oracle

使用场景：使用mybatis的Batch方式，插入数据库，造成ORA-00064异常



原因解析：

使用批量方式时，不断地增加statements中的paraments的数量，导致oracle的 stack 溢出；

oracle每一个statements中的参数值是有限制量的。



参考：

https://stackoverflow.com/questions/32649759/using-foreach-to-do-batch-insert-with-mybatis

https://stackoverflow.com/questions/6581573/what-are-the-max-number-of-allowable-parameters-per-database-provider-type/49379324

https://docs.oracle.com/cd/B10501_01/appdev.920/a96624/e_limits.htm#LNPLS018

https://www.codota.com/code/java/methods/org.apache.ibatis.session.SqlSession/flushStatements





### Pair的动态写法

思考：该写法本身有点灵活，但是要注意数据库的解析情况

代码写法非常之简介，非常优秀并且很灵活



但其本质的动态思想，反而不太像是mybatis的xml的半持久化的写法了，本身的结构都是固定的，使用字段仅仅是看着比较长，但是能一眼看出来具体内部的字段信息

```java
@POST
@Path("/getDemo")
public DemoBo getDemo(@QueryParam("id") String id, @QueryParam("name") String name) {

    List<String> inputs = Arrays.asList(id, name);
    Iterator<String> iterator = inputs.iterator();
    List<Pair<String, String>> params = DemoBo.getFields().stream()
        .map(field -> fieldToPair(field, iterator))
        .collect(Collectors.toList());

    demoDao.mergeDemoInfoById(params,"ID");

    return new DemoBo(id, name);
}

private static Pair<String, String> fieldToPair(Field field, Iterator<String> iterator) {
    Column column = field.getAnnotation(Column.class);
    if (ObjectUtil.isEmpty(column)) {
        throw new EbdcException(EbdcErrorCode.Biz.B0001);
    }
    return new Pair<>(column.name(), iterator.next());
}
```



```java
public class DemoBo {

    private static final List<Field> fields = Arrays.stream(DemoBo.class.getFields())
            .filter(i -> !Modifier.isStatic(i.getModifiers()))
            .collect(Collectors.toList());

    public static List<Field> getFields() {
        return fields;
    }

    public DemoBo(String id, String name) {
        this.id = id;
        this.name = name;
    }

    @Column(name = "ID")
    public String id;

    @Column(name = "NAME")
    public String name;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cebbank.ebdc.dao.DemoDao">
    <update id="mergeDemoInfoById" parameterType="javafx.util.Pair">
        merge into DEMO T1 using (select
        <foreach collection="params" item="demo" separator=",">
            #{demo.value} as ${demo.key}
        </foreach>
        from dual) T2
        on ( T1.${matchCondition} = T2.${matchCondition} )
        when matched then
        update
        set
        <foreach collection="params" item="demo" separator=",">
            <if test="demo.key != matchCondition">T1.${demo.key} = T2.${demo.key}</if>
        </foreach>
        when not matched then
        insert(
        <foreach collection="params" item="demo" separator=",">
            ${demo.key}
        </foreach>
        )
        values (
        <foreach collection="params" item="demo" separator=",">
            T2.${demo.key}
        </foreach>
        )
    </update>
</mapper>

```



# Cursor使用

简单的demo实例如下：

* Service服务层及如下代码，需要保证，在使用`Cursor`时，要保障游标在事务内部的使用；
    * 在 Spring 中，我们可以用 TransactionTemplate 来执行一个数据库事务，这个过程中数据库连接同样是打开的；
    * 用 SqlSessionFactory 来手工打开数据库连接；
    * 使用 `@Transactional` 注解操作。

```java
public interface DemoService {

    void testCursor(Integer limit);

}
```

```java
@Service
@Transactional
public class DemoServiceImpl implements DemoService {

    private static final CSPSLogger logger = CSPSLogFactory.get(DemoServiceImpl.class);

    private DemoDao dao;

    @Autowired
    public void setDao(DemoDao dao) {
        this.dao = dao;
    }

    @Override
    @Transactional
    public void testCursor(Integer limit) {
        try (Cursor<Map<String, String>> cursor = dao.scan(limit)) {
            cursor.forEach(System.out::println);
        } catch (IOException e) {
            logger.error("输出异常", e);
        }
    }
}
```



Dao的实现规则原理

```java
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.cursor.Cursor;

import java.util.Map;

public interface DemoDao {

    Cursor<Map<String, String>> scan(@Param("limit") Integer limit);
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cebbank.ebdc.product.mobilefund.dao.DemoDao">
    <select id="scan" parameterType="java.lang.Integer" resultType="java.util.Map">
        select *
        from DEMO
        where rownum &lt;= #{limit}
    </select>

</mapper>
```

参考：https://developer.aliyun.com/article/780643

