# JavaScript 调试



## JavaScript 调试器

查找编程代码中的错误被称为代码调试。

调试并不简单。但幸运地是，所有现代浏览器都有内置的调试器。

内置的调试器可打开或关闭，强制将错误报告给用户。

通过调试器，您也可以设置断点（代码执行被中断的位置），并在代码执行时检查变量。

通常通过 F12 键启动浏览器中的调试器，然后在调试器菜单中选择“控制台”。



## console.log() 方法

如果您的浏览器支持调试，那么您可以使用 console.log() 在调试窗口中显示 JavaScript 的值：

实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>My First Web Page</h1>

<script>
a = 5;
b = 6;
c = a + b;
console.log(c);
</script>

</body>
</html>
```



## debugger 关键词

*debugger* 关键词会停止 JavaScript 的执行，并调用（如果有）调试函数。

这与在调试器中设置断点的功能是一样的。

如果调试器不可用，debugger 语句没有效果。

如果调试器已打开，此代码会在执行第三行之前停止运行。

### 实例

```javascript
var x = 15 * 5;
debugger;
document.getElementbyId("demo").innerHTML = x; 
```





## 运算符周围的空格

请始终在运算符（ = + - * / ）周围以及逗号之后添加空格：

### 实例

```javascript
var x = y + z;
var values = ["Volvo", "Saab",  "Fiat"];
```



## 代码缩进

请始终使用对代码块缩进使用 4 个空格：

函数

```javascript
function toCelsius(fahrenheit) {
    return (5 / 9) * (fahrenheit - 32);
}
```



## 对象规则

针对对象定义的通用规则：

- 把开括号与对象名放在同一行
- 在每个属性与其值之间使用冒号加一个空格
- 不要在最后一个属性值对后面写逗号
- 请在新行上写闭括号，不带前导空格
- 请始终以分号结束对象定义

实例

```javascript
var person = {
    firstName: "Bill",
    lastName: "Gates",
    age: 50,
    eyeColor:  "blue"
};
```



## 在 HTML 中加载 JavaScript

使用简单的语法来加载外部脚本（type 属性不是必需的）：

```html
<script src="myscript.js"></script>
```



# JavaScript 最佳实践

**请避免全局变量、new、===、eval()**



# JavaScript 常见错误



## 意外使用赋值运算符

这条 if 语句返回 true（也许不像预期），因为 10 为 true：

```javascript
var x = 0;
if (x = 10) 
```



## 令人困惑的加法和级联

*加法*用于加*数值*。

*级联（Concatenation）*用于加*字符串*。

在 JavaScript 中，这两种运算均使用相同的 + 运算符。

正因如此，将数字作为数值相加，与将数字作为字符串相加，将产生不同的结果：

```javascript
var x = 10 + 5;          // x 中的结果是 15
var x = 10 + "5";         // x 中的结果是　"105"
```



## 令人误解的浮点

JavaScript 中的数字均保存为 64 位的*浮点数（Floats）*。

所有编程语言，包括 JavaScript，都存在处理浮点值的困难：

```javascript
var x = 0.1;
var y = 0.2;
var z = x + y             // z 中的结果并不是 0.3
```

为了解决上面的问题，请使用乘除运算：

实例

```javascript
var z = (x * 10 + y * 10) / 10;       // z 中的结果将是 0.3
```



## 对 JavaScript 字符串换行

如果必须在字符串中换行，则必须使用反斜杠：

### 例子 3

```
var x = "Hello \
World!";
```



## 对 return 语句进行换行

但是，如果把 return 语句换行为两行会发生什么呢：

### 例子 4

```html
<!DOCTYPE html>
<html>
<body>

<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML = myFunction(55);
function myFunction(a) {
  var
  power = 10;
  return a * power;
}
</script>

</body>
</html>

```



## 通过命名索引来访问数组

很多编程语言支持带有命名索引的数组。

带有命名索引的数组被称为关联数组（或散列）。

JavaScript *不支持*带有命名索引的数组。

在 JavaScript 中，*数组*使用*数字索引*：

实例

```javascript
var person = [];
person[0] = "Bill";
person[1] = "Gates";
person[2] = 46;
var x = person.length;          // person.length 将返回 3
var y = person[0];              // person[0] 将返回 "Bill"
```



## Undefined 不是 Null

JavaScript 对象、变量、属性和方法可以是未定义的。

此外，空的 JavaScript 对象的值可以为 null。

这可能会使测试对象是否为空变得有点困难。

您可以通过测试类型是否为 undefined，来测试对象是否存在：

实例

```javascript
if (typeof myObj === "undefined")
```





# JavaScript 性能

**如何加速您的 JavaScript 代码。**

## 减少循环中的活动

## 减少 DOM 访问

与其他 JavaScript 相比，访问 HTML DOM 非常缓慢。

假如您期望访问某个 DOM 元素若干次，那么只访问一次，并把它作为本地变量来使用：

实例

```javascript
var obj;
obj = document.getElementById("demo");
obj.innerHTML = "Hello"; 
```

## 避免不必要的变量

请不要创建不打算存储值的新变量。

通常您可以替换代码：

```
var fullName = firstName + " " + lastName;
document.getElementById("demo").innerHTML = fullName; 
```

用这段代码：

```javascript
document.getElementById("demo").innerHTML = firstName + " " + lastName
```



## 延迟 JavaScript 加载

请把脚本放在页面底部，使浏览器首先加载页面。

脚本在下载时，浏览器不会启动任何其他的下载。此外所有解析和渲染活动都可能会被阻塞。

HTTP 规范定义浏览器不应该并行下载超过两种要素。

一个选项是在 script 标签中使用 defer="true"。defer 属性规定了脚本应该在页面完成解析后执行，但它只适用于外部脚本。

如果可能，您可以在页面完成加载后，通过代码向页面添加脚本;





# JavaScript 保留词

## JavaScript 保留词

在 JavaScript 中，您不能把这些保留词作为变量、标记或函数名来使用：

| abstract | arguments  | await*       | boolean   |
| -------- | ---------- | ------------ | --------- |
| break    | byte       | case         | catch     |
| char     | class*     | const        | continue  |
| debugger | default    | delete       | do        |
| double   | else       | enum*        | eval      |
| export*  | extends*   | false        | final     |
| finally  | float      | for          | function  |
| goto     | if         | implements   | import*   |
| in       | instanceof | int          | interface |
| let*     | long       | native       | new       |
| null     | package    | private      | protected |
| public   | return     | short        | static    |
| super*   | switch     | synchronized | this      |
| throw    | throws     | transient    | true      |
| try      | typeof     | var          | void      |
| volatile | while      | with         | yield     |



## JavaScript 对象、属性和方法

您还应该避免使用 JavaScript 内建对象的名称、属性和方法：

| Array          | Date     | eval      | function  |
| -------------- | -------- | --------- | --------- |
| hasOwnProperty | Infinity | isFinite  | isNaN     |
| isPrototypeOf  | length   | Math      | NaN       |
| name           | Number   | Object    | prototype |
| String         | toString | undefined | valueOf   |



## Java 保留词

JavaScript 常与 Java 一起使用。您应该避免把某些 Java 对象和属性用作 JavaScript 标识符：

| getClass   | java        | JavaArray | javaClass |
| ---------- | ----------- | --------- | --------- |
| JavaObject | JavaPackage |           |           |



## HTML 事件处理程序

此外您应该避免使用所有 HTML 事件处理程序的名称。

例如：

| onblur    | onclick    | onerror     | onfocus     |
| --------- | ---------- | ----------- | ----------- |
| onkeydown | onkeypress | onkeyup     | onmouseover |
| onload    | onmouseup  | onmousedown | onsubmit    |



# ECMAScript 5 - JavaScript 5



## ECMAScript 5 特性

这些是 2009 年发布的新特性：

- "use strict" 指令
- String.trim()
- Array.isArray()
- Array.forEach()
- Array.map()
- Array.filter()
- Array.reduce()
- Array.reduceRight()
- Array.every()
- Array.some()
- Array.indexOf()
- Array.lastIndexOf()
- JSON.parse()
- JSON.stringify()
- Date.now()
- 属性 Getter 和 Setter
- 新的对象属性和方法

https://www.w3school.com.cn/js/js_es5.asp



## ECMAScript 5 语法更改

- 对字符串的属性访问 [ ]
- 数组和对象字面量中的尾随逗号
- 多行字符串字面量
- 作为属性名称的保留字





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

```
{
"employees":[
    {"firstName":"Bill", "lastName":"Gates"}, 
    {"firstName":"Steve", "lastName":"Jobs"},
    {"firstName":"Alan", "lastName":"Turing"}
]
}
```



## 把 JSON 文本转换为 JavaScript 对象

JSON 的通常用法是从 web 服务器读取数据，然后在网页中显示数据。

为了简单起见，可以使用字符串作为输入演示。

首先，创建包含 JSON 语法的 JavaScript 字符串：

```
var text = '{ "employees" : [' +
'{ "firstName":"Bill" , "lastName":"Gates" },' +
'{ "firstName":"Steve" , "lastName":"Jobs" },' +
'{ "firstName":"Alan" , "lastName":"Turing" } ]}';
```

然后，使用 JavaScript 的内建函数 JSON.parse() 来把这个字符串转换为 JavaScript 对象：

```
var obj = JSON.parse(text);
```

最后，请在您的页面中使用这个新的 JavaScript 对象：

```html
<!DOCTYPE html>
<html>
<body>

<h1>用 JSON 字符串创建对象</h1>

<p id="demo"></p>

<script>
var text = '{"employees":[' +
'{"firstName":"Bill","lastName":"Gates" },' +
'{"firstName":"Steve","lastName":"Jobs" },' +
'{"firstName":"Elon","lastName":"Musk" }]}';

obj = JSON.parse(text);
document.getElementById("demo").innerHTML =
obj.employees[1].firstName + " " + obj.employees[1].lastName;
</script>

</body>
</html>
```

