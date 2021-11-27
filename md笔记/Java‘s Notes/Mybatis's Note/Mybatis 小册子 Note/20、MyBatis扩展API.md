个人总结：

1.   Reflector反射工具类；

2.   链式的迭代器设计 - PropertyTokenizer；

3.   MetaClass，ObjectWrapper（...），MetaObject 类信息到实例信息的转换；

4.   在ParameterHandler中，

     ①通过MetaObject获取到对应的实例value

     ②然后通过Mapper获取TypeHandler

     ③最后进行数据的转换

