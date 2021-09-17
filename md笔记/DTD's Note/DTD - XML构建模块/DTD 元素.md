## 声明一个元素

在 DTD 中，XML 元素通过元素声明来进行声明。元素声明使用下面的语法：

```dtd
<!ELEMENT element-name category>
```

或

```dtd
<!ELEMENT element-name (element-content)>
```





## 空元素

空元素通过类别关键词EMPTY进行声明：

```dtd
<!ELEMENT element-name EMPTY>
```

实例:

```dtd
<!ELEMENT br EMPTY>
```

XML example:

```xml
<br />
```





## 只有 PCDATA 的元素

只有 PCDATA 的元素通过圆括号中的 #PCDATA 进行声明：

```dtd
<!ELEMENT element-name (#PCDATA)>
```

实例:

```dtd
<!ELEMENT from (#PCDATA)>
```





## 带有任何内容的元素

通过类别关键词 ANY 声明的元素，可包含任何可解析数据的组合：

```dtd
<!ELEMENT element-name ANY>
```

实例:

```dtd
<!ELEMENT note ANY>
```





## 带有子元素（序列）的元素

带有一个或多个子元素的元素通过圆括号中的子元素名进行声明：

```dtd
<!ELEMENT element-name (child1)>
```

或

```dtd
<!ELEMENT element-name (child1,child2,...)>
```

实例:

```dtd
<!ELEMENT note (to,from,heading,body)>
```



Example:

```dtd
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
```





## 声明只出现一次的元素

```dtd
<!ELEMENT element-name (child-name)>
```

实例:

```dtd
<!ELEMENT note (message)>
```





## 声明最少出现一次的元素

```dtd
<!ELEMENT element-name (child-name+)>
```

实例:

```dtd
<!ELEMENT note (message+)>
```

上面的例子中的加号（+）声明了：message 子元素必须在 "note" 元素内出现至少一次。





## 声明出现零次或多次的元素

```dtd
<!ELEMENT element-name (child-name*)>
```

实例:

```dtd
<!ELEMENT note (message*)>
```

上面的例子中的星号（*）声明了：子元素 message 可在 "note" 元素内出现零次或多次。





## 声明出现零次或一次的元素

```dtd
<!ELEMENT element-name (child-name?)>
```

实例:

```dtd
<!ELEMENT note (message?)>
```

上面的例子中的问号(?)声明了：子元素 message 可在 "note" 元素内出现零次或一次。





## 声明"非.../即..."类型的内容

实例:

```dtd
<!ELEMENT note (to,from,header,(message|body))>
```

上面的例子声明了："note" 元素必须包含 "to" 元素、"from" 元素、"header" 元素，以及非 "message" 元素即 "body" 元素。





## 声明混合型的内容

实例:

```dtd
<!ELEMENT note (#PCDATA|to|from|header|message)*>
```

上面的例子声明了："note" 元素可包含出现零次或多次的 PCDATA、"to"、"from"、"header" 或者 "message"。





注意⚠️ - dtd文件中，规定了元素的顺序

例如mybatis中的dtd文件 - mybatis-3-config.dtd

```dtd
<!ELEMENT configuration (properties?, settings?, typeAliases?, typeHandlers?, objectFactory?, objectWrapperFactory?, reflectorFactory?, plugins?, environments?, databaseIdProvider?, mappers?)>
```

规定了在应用的setting文件中，dtd文件的先后顺序，不按照顺序进行声明，xml的configuration标签，会进行错误提示

