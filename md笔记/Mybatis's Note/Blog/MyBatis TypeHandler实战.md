MyBatis的TypeHandler



MyBatis TypeHandler实战

 

MyBatis在金融领域的使用非常广泛，作为一款优秀的ORM框架，其独特的优势在于：

①对于sql的分离，使业务开发同事更专注于逻辑实现；

②MyBatis对存储过程的有很好的支持（特别是对一些时间延时要求高，业务较为稳定的场景）。



作为一款ORM框架，不同的类型之间数据的转换就是MyBatis首先要考虑的；一项基本的功能，就是帮助我们，将JDBC类型与Java类型之间转换变得透明，能解放大家更多得精力去与产品吵架（狗头）。开个玩笑，正是由于JDBC类型与Java类型，并不是一对一得关系，所以我们更加需要MyBatis去帮我们解决这类繁琐而且重复的工作。 



TypeHandler，就是MyBatis给出的解决方案。





