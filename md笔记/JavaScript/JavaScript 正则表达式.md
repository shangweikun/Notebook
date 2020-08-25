# JavaScript 正则表达式

## 使用字符串方法

在 JavaScript 中，正则表达式常用于两个*字符串方法*：search() 和 replace()。

search() 方法使用表达式来搜索匹配，然后返回匹配的位置。

replace() 方法返回模式被替换处修改后的字符串。



## 在字符串方法 search() 中使用正则表达式

### 实例

使用正则表达式执行搜索字符串中 "w3school" 的大小写不敏感的搜索：

```javascript
var str = "Visit W3School";
var n = str.search(/w3school/i); 
```

n 中的结果将是：

```
6
```



## 正则表达式修饰符

*修饰符*可用于大小写不敏感的更全局的搜素：

| 修饰符 | 描述                                                     |                                                              |
| :----- | :------------------------------------------------------- | ------------------------------------------------------------ |
| i      | 执行对大小写不敏感的匹配。                               | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_i) |
| g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_g) |
| m      | 执行多行匹配。                                           | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_m) |



## 正则表达式模式

*括号*用于查找一定范围的字符串：

| 表达式 | 描述                       |                                                              |
| :----- | :------------------------- | ------------------------------------------------------------ |
| [abc]  | 查找方括号之间的任何字符。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_abc) |
| [0-9]  | 查找任何从 0 至 9 的数字。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_0-9) |
| (x\|y) | 查找由 \| 分隔的任何选项。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_xy) |

```javascript
//a,b,c 任意匹配
document.getElementById("demo2").innerHTML = "123b123a1231231".search(/[abc]/);
document.getElementById("demo2").innerHTML = "123b123a1231231".search(/(a|b|c)/);

//0-9 任意匹配一个
document.getElementById("demo3").innerHTML = "123123a1231231".search(/[0-9]/);

//匹配 1-9 和 a或b 
document.getElementById("demo4").innerHTML = "02b3123a1231231".search(/[1-9](a|b)]/);

//1-9或（a或b）
document.getElementById("demo4").innerHTML = "02b3123a1231231".search(/[(1-9)(a|b)]/);

```



*元字符（Metacharacter）*是拥有特殊含义的字符：

| 元字符 | 描述                                        |                                                              |
| :----- | :------------------------------------------ | ------------------------------------------------------------ |
| \d     | 查找数字。                                  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_d) |
| \s     | 查找空白字符。                              | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_s) |
| \b     | 匹配单词边界。                              | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_b) |
| \uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_ux) |

```javascript
document.getElementById("demo4").innerHTML = "02b3123a1231231".search(/\d(a|b)/);
```



*Quantifiers* 定义量词：

| 量词 | 描述                                |                                                              |
| :--- | :---------------------------------- | ------------------------------------------------------------ |
| n+   | 匹配任何包含至少一个 n 的字符串。   | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_n_1) |
| n*   | 匹配任何包含零个或多个 n 的字符串。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_n_2) |
| n?   | 匹配任何包含零个或一个 n 的字符串。 | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_regexp_n_3) |



## 使用 RegExp 对象

在 JavaScript 中，RegExp 对象是带有预定义属性和方法的正则表达式对象。

## 使用 test()

test() 是一个正则表达式方法。

它通过模式来搜索字符串，然后根据结果返回 true 或 false。

下面的例子搜索字符串中的字符 "e"：

### 实例

```javascript
var patt = /e/;
patt.test("The best things in life are free!"); 
```

由于字符串中有一个 "e"，以上代码的输出将是：

```
true
```

您不必首先把正则表达式放入变量中。上面的两行可缩短为一行：

```javascript
/e/.test("The best things in life are free!");
```



## 使用 exec()

exec() 方法是一个正则表达式方法。

它通过指定的模式（pattern）搜索字符串，并返回已找到的文本。

如果未找到匹配，则返回 null。下面的例子搜索字符串中的字符 "e"：

### 实例

```javascript
/e/.exec("The best things in life are free!");
```

由于字符串中有一个 "e"，以上代码的输出将是：

```
e
```

 [JavaScript RegExp 参考手册](https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp)



