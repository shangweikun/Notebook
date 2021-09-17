# JavaScript JSON



**JSON 是存储和传输数据的格式。**

**JSON 经常在数据从服务器发送到网页时使用。**



## 什么是 JSON？

- JSON 指的是 **J**ava**S**cript **O**bject **N**otation
- JSON 是轻量级的数据交换格式
- JSON 独立于语言 *****
- JSON 是“自描述的”且易于理解

\* JSON 的语法是来自 JavaScript 对象符号的语法，但 JSON 格式是纯文本。读取和生成 JSON 数据的代码可以在任何编程语言编写的。



## JSON 实例

JSON 语法定义了一个雇员对象：包含三条员工记录的数组（对象）：

### JSON 实例

```javascript
{
"employees":[
    {"firstName":"Bill", "lastName":"Gates"}, 
    {"firstName":"Steve", "lastName":"Jobs"},
    {"firstName":"Alan", "lastName":"Turing"}
]
}
```

## JSON 格式评估为 JavaScript 对象

JSON 格式在语法上与创建 JavaScript 对象的代码相同。

由于这种相似性，JavaScript 程序可以很容易地将 JSON 数据转换成本地的 JavaScript 对象。

## JSON 语法规则

- 数据是名称/值对
- 数据由逗号分隔
- 花括号保存对象
- 方括号保存数组



## JSON 数据 - 名称和值

JSON 数据的书写方式是名称/值对，类似 JavaScript 对象属性。

名称/值对由（双引号中的）字段名构成，其后是冒号，再其后是值：

```javascript
"firstName":"Bill"
```

JSON 名称需要双引号。JavaScript 名称不需要。



## JSON 对象

JSON 对象是在花括号内书写的。

类似 JavaScript，对象能够包含多个名称/值对：

```javascript
{"firstName":"Bill", "lastName":"Gates"} 
```



## JSON 数组

JSON 数组在方括号中书写。

类似 JavaScript，数组能够包含对象：

```javascript
"employees":[
    {"firstName":"Bill", "lastName":"Gates"}, 
    {"firstName":"Steve", "lastName":"Jobs"}, 
    {"firstName":"Alan", "lastName":"Turing"}
]
```

在上面的例子中，对象 "employees" 是一个数组。它包含了三个对象。

每个对象代表一个人的一条记录（带有名和姓）。



## 把 JSON 文本转换为 JavaScript 对象

JSON 的通常用法是从 web 服务器读取数据，然后在网页中显示数据。

为了简单起见，可以使用字符串作为输入演示。

首先，创建包含 JSON 语法的 JavaScript 字符串：

```javascript
var text = '{ "employees" : [' +
'{ "firstName":"Bill" , "lastName":"Gates" },' +
'{ "firstName":"Steve" , "lastName":"Jobs" },' +
'{ "firstName":"Alan" , "lastName":"Turing" } ]}';
```

然后，使用 JavaScript 的内建函数 JSON.parse() 来把这个字符串转换为 JavaScript 对象：

```javascript
var obj = JSON.parse(text);
```

最后，请在您的页面中使用这个新的 JavaScript 对象：

### 实例

```html
<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML =
obj.employees[1].firstName + " " + obj.employees[1].lastName;
</script> 
```

