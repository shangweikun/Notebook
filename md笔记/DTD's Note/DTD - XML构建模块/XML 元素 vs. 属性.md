## 使用元素 vs. 属性

数据可以存储在子元素或属性。

让我们来看下这些实例:

```xml
<person sex="female">
 <firstname>Anna</firstname>
 <lastname>Smith</lastname>
</person>
```



```xml
<person>
 <sex>female</sex>
 <firstname>Anna</firstname>
 <lastname>Smith</lastname>
</person>
```



## 避免使用属性?

你应该避免使用属性?

一些属性具有以下问题:

- 属性不能包含多个值（子元素可以）
- 属性不容易扩展（为以后需求的变化）
- 属性无法描述结构（子元素可以）
- 属性更难以操纵程序代码
- 属性值是不容易测试，针对DTD

如果您使用属性作为数据容器，最终的XML文档将难以阅读和维护。 尝试使用**元素**来描述数据。只有在提供的数据是不相关信息时我们才建议使用属性。

不要这个样子结束（这不是XML应该使用的）：

```xml
<note day="12" month="11" year="2002"
to="Tove" from="Jani" heading="Reminder"
body="Don't forget me this weekend!">
</note>
```





## 一个属性规则的例外

规则总是有另外的

关于属性的规则我有一个例外情况。

有时我指定的 ID 应用了元素。这些 ID 应用可在HTML中的很多相同的情况下可作为 NAME 或者 ID 属性来访问 XML 元素。以下实例展示了这种方式：

```xml
<messages>
<note id="p501">
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>

<note id="p502">
  <to>Jani</to>
  <from>Tove</from>
  <heading>Re: Reminder</heading>
  <body>I will not!</body>
</note>
</messages>
```

以上实例的XML文件中，ID是只是一个计数器，或一个唯一的标识符，来识别不同的音符，而不是作为数据的一部分。

在这里我想说的是，元数据（关于数据的数据）应当存储为属性，而数据本身应当存储为元素。





# DTD - 实体

------

实体是用于定义引用普通文本或特殊字符的快捷方式的变量。

- 实体引用是对实体的引用。
- 实体可在内部或外部进行声明。



## 一个内部实体声明

### 语法

```dtd
<!ENTITY entity-name "entity-value">
```

### 实例

DTD 实例:

```dtd
<!ENTITY writer "Donald Duck.">
<!ENTITY copyright "Copyright runoob.com">
```

XML 实例：

```xml-dtd
<author>&writer;&copyright;</author>
```

**注意：** 一个实体由三部分构成: 一个和号 (&), 一个实体名称, 以及一个分号 (;)。





## 一个外部实体声明

### 语法

```dtd
<!ENTITY entity-name SYSTEM "URI/URL">
```

### 实例

DTD 实例:

```dtd
<!ENTITY writer SYSTEM "http://www.runoob.com/entities.dtd">
<!ENTITY copyright SYSTEM "http://www.runoob.com/entities.dtd">
```

XML example:

```dtd
<author>&writer;&copyright;</author>
```





