# 技巧

##IDEA类和方法注释模板设置（非常详细）

File-->settings-->Editor-->File and Code Templates-->Files

我们选择Class文件（当然你要设置接口的还也可以选择Interface文件）

```txt
（1）${NAME}：设置类名，与下面的${NAME}一样才能获取到创建的类名

（2）TODO：代办事项的标记，一般生成类或方法都需要添加描述

（3）${USER}、${DATE}、${TIME}：设置创建类的用户、创建的日期和时间，这些事IDEA内置的方法，还有一些其他的方法在绿色框标注的位置，比如你想添加项目名则可以使用${PROJECT_NAME}

（4）1.0：设置版本号，一般新创建的类都是1.0版本，这里写死就可以了
```



设置方法注释模板

IDEA还没有智能到自动为我们创建方法注释，这就是要我们手动为方法添加注释，使用Eclipse时我们生成注释的习惯是

/**+Enter，这里我们也按照这种习惯来设置IDEA的方法注释

1、File-->Settings-->Editor-->Live Templates

![img](https://img-blog.csdn.net/20180111101422713?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（1）新建组：命名为userDefine

![img](https://img-blog.csdn.net/20180111101509270?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（2）新建模板：命名为*

因为IDEA生成注释的默认方式是：/*+模板名+快捷键（比如若设置模板名为add快捷键用Tab，则生成方式为

/*add+Tab），如果不采用这样的生成方式IDEA中没有内容的方法将不可用，例如获取方法参数的methodParameters(）、

获取方法返回值的methodReturnType(）

![img](https://img-blog.csdn.net/20180111101620220?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（3）设置生成注释的快捷键

![img](https://img-blog.csdn.net/20180111102440886?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（4）设置模板：模板内容如下

注意第一行，只有一个*而不是/*

在设置参数名时必须用${参数名}$的方式，否则第五步中读取不到你设置的参数名

```java
*



 * @Author chengpunan



 * @Description //TODO $end$



 * @Date $time$ $date$



 * @Param $param$



 * @return $return$



 **/
```

如果使用/*生成的模板注释将会是如下效果：所以我们要去掉最前面的/*

![img](https://img-blog.csdn.net/20180111102738652?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（5）设置模板的应用场景

点击模板页面最下方的警告，来设置将模板应用于那些场景，一般选择EveryWhere-->Java即可

（如果曾经修改过，则显示为change而不是define）

![img](https://img-blog.csdn.net/20180111103636249?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![img](https://img-blog.csdn.net/20180111103825818?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（6）设置参数的获取方式

选择右侧的Edit variables按钮

PS:第五步和第六步顺序不可颠倒，否则第六步将获取不到方法

![img](https://img-blog.csdn.net/20180111102925939?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

选择每个参数对应的获取方法（在下拉选择框中选择即可），网上有很多教程说获取param时使用脚本的方式，我试过使用脚本

的方式不仅麻烦而且只能在方法内部使用注释时才能获取到参数

![img](https://img-blog.csdn.net/20180111103058459?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（7）效果图

创建方法，在方法上面写：/*+模板名+Enter-->/**+Enter

![img](https://img-blog.csdn.net/20180111104034543?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2xpdWxhbmcwMzI0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

## 官方链接 - Ideal

The following example shows the default template for creating a Java class in IntelliJ IDEA:

```velocity
#if (${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("File Header.java")
public class ${NAME} {
}
```

Ideal 官方链接：https://www.jetbrains.com/help/idea/using-file-and-code-templates.html#syntax



##Ideal依赖本地其他项目

