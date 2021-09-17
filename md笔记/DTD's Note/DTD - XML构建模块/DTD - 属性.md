## 声明属性

属性声明使用下列语法：

```dtd
<!ATTLIST element-name attribute-name attribute-type attribute-value>
```

DTD 实例:

```dtd
<!ATTLIST payment type CDATA "check">
```

XML 实例:

```xml
<payment type="check" />
```



| 类型               | 描述                          |
| :----------------- | :---------------------------- |
| CDATA              | 值为字符数据 (character data) |
| (*en1*\|*en2*\|..) | 此值是枚举列表中的一个值      |
| ID                 | 值为唯一的 id                 |
| IDREF              | 值为另外一个元素的 id         |
| IDREFS             | 值为其他 id 的列表            |
| NMTOKEN            | 值为合法的 XML 名称           |
| NMTOKENS           | 值为合法的 XML 名称的列表     |
| ENTITY             | 值是一个实体                  |
| ENTITIES           | 值是一个实体列表              |
| NOTATION           | 此值是符号的名称              |
| xml:               | 值是一个预定义的 XML 值       |

默认**属性值**可使用下列值 :

| 值           | 解释           |
| :----------- | :------------- |
| 值           | 属性的默认值   |
| #REQUIRED    | 属性值是必需的 |
| #IMPLIED     | 属性不是必需的 |
| #FIXED value | 属性值是固定的 |



## 默认属性值

DTD:

```dtd
<!ELEMENT square EMPTY>
<!ATTLIST square width CDATA "0">
```

合法的 XML:

```dtd
<square width="100" />
```

在上面的例子中，"square" 被定义为带有 CDATA 类型的 "width" 属性的空元素。如果宽度没有被设定，其默认值为0 。





## #REQUIRED

### 语法

```dtd
<!ATTLIST element-name attribute-name attribute-type #REQUIRED>
```

### 实例

DTD:

```dtd
<!ATTLIST person number CDATA #REQUIRED>
```

合法的 XML:

```xml-dtd
<person number="5677" />
```

非法的 XML:

```xml-dtd
<person />
```

假如您没有默认值选项，但是仍然希望强制作者提交属性的话，请使用关键词 #REQUIRED。





## #IMPLIED

### 语法

```dtd
<!ATTLIST element-name attribute-name attribute-type #IMPLIED>
```

### 实例

DTD:

```dtd
<!ATTLIST contact fax CDATA #IMPLIED>
```

合法的 XML:

```xml-dtd
<contact fax="555-667788" />
```

合法的 XML:

```xml
<contact />
```

假如您不希望强制作者包含属性，并且您没有默认值选项的话，请使用关键词 #IMPLIED。





## #FIXED

### 语法

```dtd
<!ATTLIST element-name attribute-name attribute-type #FIXED "value">
```

### 实例

DTD:

```xml-dtd
<!ATTLIST sender company CDATA #FIXED "Microsoft">
```

合法的 XML:

```xml
<sender company="Microsoft" />
```

非法的 XML:

```xml
<sender company="W3Schools" />
```

如果您希望属性拥有固定的值，并不允许作者改变这个值，请使用 #FIXED 关键词。如果作者使用了不同的值，XML 解析器会返回错误。





## 列举属性值

### 语法

```dtd
<!ATTLIST element-name attribute-name (en1|en2|..) default-value>
```

### 实例

DTD:

```dtd
<!ATTLIST payment type (check|cash) "cash">
```

XML 例子:

```xml
<payment type="check" />
```

或

```xml
<payment type="cash" />
```



如果您希望属性值为一系列固定的合法值之一，请使用列举属性值。

