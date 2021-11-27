### MyBatis TypeHandler学习及实战

MyBatis在金融领域的使用非常广泛，作为一款优秀的ORM框架，其独特的优势在于：

​	①对于sql的分离，使业务开发同事更专注于逻辑实现；

​	②MyBatis对存储过程的有很好的支持（特别是对一些时间延时要求高，业务较为稳定的场景）。



作为一款ORM框架，不同的系统之间数据类型的转换就是首先要考虑的；MyBatis中的一项基本的功能，就是帮助我们，将JDBC类型与Java类型之间转换变得透明，能解放大家更多得精力去与产品吵架（狗头）。开个玩笑，正是由于JDBC类型与Java类型，并不是一对一得关系，所以我们更加需要MyBatis去帮我们解决这类繁琐而且重复的工作。 



TypeHandler，就是MyBatis给出的解决方案。



#### TypeHandler简述

MyBatis在设置预处理语句（PreparedStatement）中的参数，或从结果集中取出一个值时， 会选择使用对应的TypeHandler类型处理器，将获取到的值以合适的方式转换成 Java 类型；MyBatis的在内部设定了很多基础的处理器，下图是从MyBatis代码中截取的，框架内部封装好的类型处理器（部分）：

![image-20211028123441030](C:\Users\shang\AppData\Roaming\Typora\typora-user-images\image-20211028123441030.png)



#### TypeHandler使用

在MyBatis框架中，为了方便开发人员，提供了以下方式使用TypeHandler：

1.   从config文件中，通过typeHandlers以及子标签typeHandler，对自定义的TypeHandler进行注册;
2.   或者通过package子标签，对TypeHandler所在包进行扫描注册;
3.   在mapper.xml文件中，在resultMap或者在参数使用中，可以显示的声明TypeHandler，如下：

```xml
<result column="phone" property="phone" 			 
        	typeHandler="com.example.typeHandle.ClientPhoneTypeHandler"/>

<update id="updatePhone">
    update users
    set phone = #{phone, typeHandler=com.example.typeHandle.ClientPhoneTypeHandler}
    where id = #{id}
</update>
```

4.   spring boot集成了MyBatis的starter，可以通过*.properties的文件进行配置

````properties
mybatis.type-handlers-package=com.snapshot2.demo.typehandler
````

PS：笔者并未找到可以通过单个注册的处理器的方法



#### TypeHandler原理

那么TypeHandler在整个生命周期中是如何加载及使用的呢？

首先是注册阶段，MyBatis会通过`XMLConfigBuilder`对配置的xml文件中的标签进行解析，其中就包含上文中提到的TypeHandler相关标签；在解析时，会调用`TypeHandlerRegistry`的注册方法，将自定义的handler加载到JVM中；如果大家有兴趣，点开这个注册类，就能发现它的构建方法中，将上文列举的一些MyBatis框架内部定义的基础handler，进行了统一的初始化，并用Map将处理器与JDBC的类型建立的关联；

接下来，在解析mapper文件时，`XMLMapperBuilder`会选择适当的处理器；在解析ResultMap和ParameterMap时，从上阶段注册的TypeHandler中，找到最合适的TypeHandler，或者从mapper文件中，读取显示声明的TypehHandler，存入`ParameterMapping`实例中；

最终语句执行阶段，会调用处理器，对参数或查询结果进行转换；对应的Sql语句在输入参数时，调用TypeHander接口的`setParameter`方法，进行入参转换操作；获取到查询结果后，会调用`getResult`的方法，对结果转换后返回。



#### 自己定义一个TypeHandler

用两个实际的例子，来分享下我的使用场景。

场景1，用户更新标识；

要求：数字名片系统的人员信息修改有三种方式：1、员工手动修改；2、管理员手动修改；3、来自行内人力资源系统信息。业务规定员工手动修改信息后，不再同步来自人力资源系统的修改信息；同时需要根据修改的信息，设置同步标志，每一种不同的信息，必须通过不同的标识位进行控制。

设计与实现：人员主表增加更新标识位字段，通过二进制位，标识每个不同种类的信息的是否同步；代码中增加专用的DTO类，表明该数据为特殊更新属性标识；新增注解和二进制位枚举，通过在DTO类属性添加注解，指定该属性对应的枚举信息；增加专用TypeHandler用于数据库中的number类型与DTO类型的转换；在mapper文件中，显示的指定具体的TypeHandler。



**Talk is cheap, show me your code.**

注解类：

````java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MappedPropertiesChangeType {

    PersonDetailChangeEnum value();

    String name();
}
````



DTO类：

````java
public class PropertiesUpdateMarkDTO implements Serializable {

    @MappedPropertiesChangeType(
            value = PersonDetailChangeEnum.EMAIL,
            name = "email")
    private int email = 0;

    @MappedPropertiesChangeType(
            value = PersonDetailChangeEnum.PHONE,
            name = "phone")
    private int phone = 0;

    @MappedPropertiesChangeType(
            value = PersonDetailChangeEnum.ICON,
            name = "icon")
    private int icon = 0;

    @MappedPropertiesChangeType(
            value = PersonDetailChangeEnum.FIXTEL,
            name = "fixTel")
    private int fixTel = 0;

    @MappedPropertiesChangeType(
            value = PersonDetailChangeEnum.DUTY,
            name = "duty")
    private int duty = 0;

    private static class Constant {
        private static final Field[] _FIELDS = PropertiesUpdateMarkDTO.class.getDeclaredFields();
    }

    public int getEmail() {
        return email;
    }

    public void setEmail(int email) {
        this.email = email;
    }

    public int getPhone() {
        return phone;
    }

    public void setPhone(int phone) {
        this.phone = phone;
    }

    public int getIcon() {
        return icon;
    }

    public void setIcon(int icon) {
        this.icon = icon;
    }

    public int getFixTel() {
        return fixTel;
    }

    public void setFixTel(int fixTel) {
        this.fixTel = fixTel;
    }

    public int getDuty() {
        return duty;
    }

    public void setDuty(int duty) {
        this.duty = duty;
    }

    public static int transformDtoToInt(PropertiesUpdateMarkDTO dto) {

        return Arrays.stream(Constant._FIELDS).map(
                field -> PropertiesUpdateMarkDTO.bitToInt(field, dto)
        ).reduce(0, Integer::sum);
    }

    public static PropertiesUpdateMarkDTO transformIntToDTO(int input) {

        PropertiesUpdateMarkDTO dto = new PropertiesUpdateMarkDTO();
        Arrays.stream(Constant._FIELDS).forEach(field -> setField(field, dto, input));
        return dto;
    }

    private static int bitToInt(Field field, PropertiesUpdateMarkDTO dto) {

        MappedPropertiesChangeType[] annotations
                = field.getAnnotationsByType(MappedPropertiesChangeType.class);

        if (annotations.length > 1) {
            throw new EbdcException(EbdcErrorCode.Biz.B0001,
                    "Do not support multiple MappedPropertiesChangeType annotation");
        }

        Method method = BeanUtil.getPropertyDescriptor(
                PropertiesUpdateMarkDTO.class,
                annotations[0].name()
        ).getReadMethod();
        return ((Integer) ReflectUtil.invoke(dto, method))
                << annotations[0].value().getDisposition();
    }

    private static void setField(Field field, PropertiesUpdateMarkDTO dto, int input) {

        MappedPropertiesChangeType[] annotations
                = field.getAnnotationsByType(MappedPropertiesChangeType.class);

        if (annotations.length > 1) {
            throw new EbdcException(EbdcErrorCode.Biz.B0001,
                    "Do not support multiple MappedPropertiesChangeType annotation");
        }

        Method method = BeanUtil.getPropertyDescriptor(
                PropertiesUpdateMarkDTO.class,
                annotations[0].name()
        ).getWriteMethod();
        ReflectUtil.invoke(dto, method,
                (input & annotations[0].value().getBinary())
                        >> annotations[0].value().getDisposition());
    }

}
````



Enum类：

````java
public enum PersonDetailChangeEnum {

    EMAIL(0),//邮箱
    PHONE(1),//移动电话
    ICON(2),//头像
    FIXTEL(3),//固定电话
    DUTY(4);//职务

    private final int binary;

    private final int disposition;

    public Integer getBinary() {
        return binary;
    }

    public int getDisposition() {
        return disposition;
    }

    PersonDetailChangeEnum(int disposition) {
        this.binary = 1 << disposition;
        this.disposition = disposition;
    }

    public static Integer changeBinaryState(List<Integer> integers, Integer result) {
        if (result == null) {
            result = 0;
        }
        for (Integer value : integers) {
            result = value | result;
        }
        return result;
    }
}
````



TypeHandler实现：

```java
@MappedTypes(PropertiesUpdateMarkDTO.class)
@MappedJdbcTypes(JdbcType.NUMERIC)
public class PropertiesMarkHandler implements TypeHandler<PropertiesUpdateMarkDTO> {

    @Override
    public void setParameter(PreparedStatement preparedStatement, int i,
                             PropertiesUpdateMarkDTO propertiesUpdateMarkDTO, JdbcType jdbcType) throws SQLException {
        preparedStatement.setInt(i,
                PropertiesUpdateMarkDTO.transformDtoToInt(propertiesUpdateMarkDTO));
    }

    @Override
    public PropertiesUpdateMarkDTO getResult(ResultSet resultSet, String s) throws SQLException {
        return PropertiesUpdateMarkDTO.transformIntToDTO(resultSet.getInt(s));
    }

    @Override
    public PropertiesUpdateMarkDTO getResult(ResultSet resultSet, int i) throws SQLException {
        return PropertiesUpdateMarkDTO.transformIntToDTO(resultSet.getInt(i));
    }

    @Override
    public PropertiesUpdateMarkDTO getResult(CallableStatement callableStatement, int i) throws SQLException {
        return PropertiesUpdateMarkDTO.transformIntToDTO(callableStatement.getInt(i));
    }
}
```



mapper.xml文件：

````xml
<resultMap id="HrsPersonVOResultMap" type="*.person.bo.PersonVO" extends="PersonVOResultMap">
   <result column="JOB_NAME" property="jobName"/>
   <result column="PROPERTIES_UPDATE_MARK" property="properties"
         typeHandler="*.PropertiesMarkHandler"/>
</resultMap>
````





场景2，银行卡号脱敏展示；

要求：银行页面上，用于展示用户银行卡账号的时，为了保证客户信息不被泄露，都必须经过脱敏处理的，显示带有****的银行卡号；

设计与实现：增加特殊的String字段的TypeHandler类，并在mapper中，将需要脱敏的DO中所对应的ResultMap中的字段，显示的指定该TypeHandler；数据库读取到的银行卡账号信息，通过`getNullableResult`方法，进行脱敏处理。



TypeHandler实现：

```java
public class DesCardNoTypeHandler extends BaseTypeHandler<String> {

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType)
            throws SQLException {
    }

    @Override
    public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
        return desCardNo(rs.getString(columnName));
    }

    @Override
    public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        return desCardNo(rs.getString(columnIndex));
    }

    @Override
    public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        return desCardNo(cs.getString(columnIndex));
    }

    private static String desCardNo(String cardNo) {
        return DesensitizedUtil.bankCard(cardNo);
    }
}
```



ResultMap信息：

```xml
<resultMap id="BaseUserCardInfo" type="com.example.entity.card.UserCard">
    <id property="userId" column="user_id"/>
    <result property="cardNo" column="card_no"/>
</resultMap>

<resultMap id="DesUserCardInfo" type="com.example.entity.card.UserCard">
    <id property="userId" column="user_id"/>
    <result property="cardNo" column="card_no" typeHandler="com.example.typeHandle.DesCardNoTypeHandler"/>
</resultMap>
```



运行结果：

````shell
Created connection 2006112337.
==>  Preparing: select user_id, card_no from user_card where user_id = ?
==> Parameters: 1(String)
<==    Columns: user_id, card_no
<==        Row: 1, 6666666666666666666
<==      Total: 1
UserCard{userId='1', cardNo='6666 **** **** *** 6666'}
Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@7792d851]
````



注意：以上代码为部分功能代码，如需复制使用，请根据实际情况进行编写（强烈推荐IDEA）



---------------

20211111更新

**Spring Boot + MyBatis 单独注入TypeHandler**



2.1以上版本，可以通过`@Component`注解，注入TypeHandler；

1.   在自定义的TypeHandler类上，通过增加`@Component`注解，加入Spring的容器管理中；

2.   在MyBatis初始化`Configuration`时，可以通过`typeHandlersProvider.getIfAvailable()`方法，读取容器中的所有`TypeHandler`，并注册进入MyBatis的框架中。



具体的代码实现`MybatisAutoConfiguration`：

````java
  public MybatisAutoConfiguration(MybatisProperties properties, 
	ObjectProvider<Interceptor[]> interceptorsProvider,
	ObjectProvider<TypeHandler[]> typeHandlersProvider,
    ObjectProvider<LanguageDriver[]>languageDriversProvider,
    ResourceLoader resourceLoader,
    ObjectProvider<DatabaseIdProvider> databaseIdProvider,
    ObjectProvider<List<ConfigurationCustomizer>> configurationCustomizersProvider) {
    this.properties = properties;
    this.interceptors = interceptorsProvider.getIfAvailable();
    this.typeHandlers = typeHandlersProvider.getIfAvailable();
    this.languageDrivers = languageDriversProvider.getIfAvailable();
    this.resourceLoader = resourceLoader;
    this.databaseIdProvider = databaseIdProvider.getIfAvailable();
    this.configurationCustomizers = configurationCustomizersProvider.getIfAvailable();
  }
````

