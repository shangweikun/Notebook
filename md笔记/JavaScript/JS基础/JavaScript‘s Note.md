# JavaScrip's Note(语法及基础类型)

## getElementById

```javascript
document.getElementById("demo")
```

内部为 ***\*id\**** 属性|同时支持 “ 和 ‘ ：例如以下的例子中的使用：

```javascript
<button type="button" onclick= "document.getElementById('demo').innerHTML = 'Hello JavaScript!'">点击我！</button>
<button type="button" onclick= 'document.getElementById("demo").innerHTML = "Hello JavaScript!"'>点击我！</button>
```



**注释：**旧的 JavaScript 例子也许会使用 **type** 属性：<script type="text/javascript">。

**注释：**type 属性不是必需的。JavaScript 是 HTML 中的默认脚本语言。

**提示：**把脚本置于 **<body>** 元素的底部，可改善显示速度，因为脚本编译会拖慢显示。



外部导入js

```javascript
<script src="myScript.js"></script>
```

HTML路径相关：https://www.w3school.com.cn/html/html_filepaths.asp



## JavaScript 显示方案

JavaScript 能够以不同方式“显示”数据：

- 使用 window.alert() 写入警告框
- 使用 document.write() 写入 HTML 输出
- 使用 innerHTML 写入 HTML 元素
- 使用 console.log() 写入浏览器控制台



**注意：**在 HTML 文档完全加载后使用 **document.write()** 将*删除所有已有的 HTML* ：

**提示：****document.write()** 方法仅用于测试。



# JavaScript 语句

**在 HTML 中，JavaScript 语句是由 web 浏览器“执行”的“指令”。**

**提示：**以分号结束语句不是必需的，但我们仍然强烈建议您这么做。



## JavaScript 空白字符

JavaScript 会忽略多个空格。您可以向脚本添加空格，以增强可读性。

下面这两行是相等的：

```javascript
var person = "Bill";
var person="Bill"; 
```

在运算符旁边（ = + - * / ）添加空格是个好习惯：

```JavaScript
var x = y + z;
```



## JavaScript 代码块

JavaScript 语句可以用花括号（{...}）组合在代码块中。

代码块的作用是定义一同执行的语句。

您会在 JavaScript 中看到成块组合在一起的语句：

### 实例

```javascript
function myFunction() {
    document.getElementById("demo").innerHTML = "Hello Kitty.";
    document.getElementById("myDIV").innerHTML = "How are you?";
}
```



*字符串*是文本，由双引号或单引号包围：

```javascript
"Bill Gates"

'Bill Gates' 
```



## JavaScript 变量

在编程语言中，*变量*用于*存储*数据值。

JavaScript 使用 var 关键词来*声明*变量。

= 号用于为变量*赋值*。

在本例中，x 被定义为变量。然后，x 被赋的值是 7：

```javascript
var x;

x = 7;
```



## JavaScript 标识符

标识符是名称。

在 JavaScript 中，标识符用于命名变量（以及关键词、函数和标签）。

在大多数编程语言中，合法名称的规则大多相同。

在 JavaScript 中，首字符必须是字母、下划线（-）或美元符号（$）。

连串的字符可以是字母、数字、下划线或美元符号。

**提示：**数值不可以作为首字符。这样，JavaScript 就能轻松区分标识符和数值。



## JavaScript 对大小写敏感

所有 JavaScript 标识符*对大小写敏感*。

变量 lastName 和 lastname，是两个不同的变量。

```javascript
lastName = "Gates";
lastname = "Jobs"; 
```



## JavaScript 与驼峰式大小写

历史上，程序员曾使用三种把多个单词连接为一个变量名的方法：

### 连字符：

```
first-name, last-name, master-card, inter-city.
```

**注释：**JavaScript 中不能使用连字符。它是为减法预留的。

### 下划线：

```
first_name, last_name, master_card, inter_city.
```

### 驼峰式大小写（Camel Case）：

```
FirstName, LastName, MasterCard, InterCity.
```

![camelCase](https://www.w3school.com.cn/i/camelcase.png)

JavaScript 程序员倾向于使用以小写字母开头的驼峰大小写：

```
firstName, lastName, masterCard, interCity
```



## 一条语句，多个变量

您可以在一条语句中声明许多变量。

以 var 作为语句的开头，并以*逗号*分隔变量：

```javascript
var person = "Bill Gates", carName = "porsche", price = 15000;
```





## Value = undefined

在计算机程序中，被声明的变量经常是不带值的。值可以是需被计算的内容，或是之后被提供的数据，比如数据输入。

不带有值的变量，它的值将是 undefined。

变量 carName 在这条语句执行后的值是 undefined：

### 实例

```javascript
var carName;
```



**提示：**如果把要给数值放入引号中，其余数值会被视作字符串并被级联。

```javascript
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 变量</h1>

<p>相加 "8" + 3 + 5 的结果是：</p>

<p id="demo"></p>

<script>
x = "8" + 3 + 5;
document.getElementById("demo").innerHTML = x;
</script>

</body>
</html>

```

```javascript
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 变量</h1>

<p>相加 3 + 5 + "8" 的结果是：</p>

<p id="demo"></p>

<script>
var x = 3 + 5 + "8"
document.getElementById("demo").innerHTML = x;
</script>

</body>
</html>
```





## JavaScript 比较运算符

| 运算符 | 描述           |
| :----- | :------------- |
| ==     | 等于           |
| ===    | 等值等型       |
| !=     | 不相等         |
| !==    | 不等值或不等型 |
| >      | 大于           |
| <      | 小于           |
| >=     | 大于或等于     |
| <=     | 小于或等于     |
| ?      | 三元运算符     |



## JavaScript 类型运算符

| 运算符     | 描述                                  |
| :--------- | :------------------------------------ |
| typeof     | 返回变量的类型。                      |
| instanceof | 返回 true，如果对象是对象类型的实例。 |



## JavaScript 位运算符

位运算符处理 32 位数。

该运算中的任何数值运算数都会被转换为 32 位的数。结果会被转换回 JavaScript 数。

| 运算符 | 描述         | 例子    | 等同于       | 结果 | 十进制 |
| :----- | :----------- | :------ | :----------- | :--- | :----- |
| &      | 与           | 5 & 1   | 0101 & 0001  | 0001 | 1      |
| \|     | 或           | 5 \| 1  | 0101 \| 0001 | 0101 | 5      |
| ~      | 非           | ~ 5     | ~0101        | 1010 | 10     |
| ^      | 异或         | 5 ^ 1   | 0101 ^ 0001  | 0100 | 4      |
| <<     | 零填充左位移 | 5 << 1  | 0101 << 1    | 1010 | 10     |
| >>     | 有符号右位移 | 5 >> 1  | 0101 >> 1    | 0010 | 2      |
| >>>    | 零填充右位移 | 5 >>> 1 | 0101 >>> 1   | 0010 | 2      |

https://www.w3school.com.cn/js/pro_js_operators_bitwise.asp



## 幂

取幂运算符（**）将第一个操作数提升到第二个操作数的幂。

### 实例

```
var x = 5;
var z = x ** 2;          // 结果是 25
```

x ** y 产生的结果与 Math.pow(x,y) 相同:

### 实例

```
var x = 5;
var z = Math.pow(x,2);   // 结果是 25
```



## JavaScript 运算符优先级值

| 值   | 运算符     | 描述             | 实例             |
| :--- | :--------- | :--------------- | :--------------- |
| 20   | ( )        | 表达式分组       | (3 + 4)          |
|      |            |                  |                  |
| 19   | .          | 成员             | person.name      |
| 19   | []         | 成员             | person["name"]   |
| 19   | ()         | 函数调用         | myFunction()     |
| 19   | new        | 创建             | new Date()       |
|      |            |                  |                  |
| 17   | ++         | 后缀递增         | i++              |
| 17   | --         | 后缀递减         | i--              |
|      |            |                  |                  |
| 16   | ++         | 前缀递增         | ++i              |
| 16   | --         | 前缀递减         | --i              |
| 16   | !          | 逻辑否           | !(x==y)          |
| 16   | typeof     | 类型             | typeof x         |
|      |            |                  |                  |
| 15   | **         | 求幂 (ES7)       | 10 ** 2          |
|      |            |                  |                  |
| 14   | *          | 乘               | 10 * 5           |
| 14   | /          | 除               | 10 / 5           |
| 14   | %          | 模数除法         | 10 % 5           |
|      |            |                  |                  |
| 13   | +          | 加               | 10 + 5           |
| 13   | -          | 减               | 10 - 5           |
|      |            |                  |                  |
| 12   | <<         | 左位移           | x << 2           |
| 12   | >>         | 右位移           | x >> 2           |
| 12   | >>>        | 右位移（无符号） | x >>> 2          |
|      |            |                  |                  |
| 11   | <          | 小于             | x < y            |
| 11   | <=         | 小于或等于       | x <= y           |
| 11   | >          | 大于             | x > y            |
| 11   | >=         | 大于或等于       | x >= y           |
| 11   | in         | 对象中的属性     | "PI" in Math     |
| 11   | instanceof | 对象的实例       | instanceof Array |
|      |            |                  |                  |
| 10   | ==         | 相等             | x == y           |
| 10   | ===        | 严格相等         | x === y          |
| 10   | !=         | 不相等           | x != y           |
| 10   | !==        | 严格不相等       | x !== y          |
|      |            |                  |                  |
| 9    | &          | 按位与           | x & y            |
| 8    | ^          | 按位 XOR         | x ^ y            |
| 7    | \|         | 按位或           | x \| y           |
| 6    | &&         | 逻辑与           | x && y           |
| 5    | \|\|       | 逻辑否           | x \|\| y         |
| 4    | ? :        | 条件             | ? "Yes" : "No"   |
|      |            |                  |                  |
| 3    | =          | 赋值             | x = y            |
| 3    | +=         | 赋值             | x += y           |
| 3    | -=         | 赋值             | x -= y           |
| 3    | *=         | 赋值             | x *= y           |
| 3    | %=         | 赋值             | x %= y           |
| 3    | <<=        | 赋值             | x <<= y          |
| 3    | >>=        | 赋值             | x >>= y          |
| 3    | >>>=       | 赋值             | x >>>= y         |
| 3    | &=         | 赋值             | x &= y           |
| 3    | ^=         | 赋值             | x ^= y           |
| 3    | \|=        | 赋值             | x \|= y          |
|      |            |                  |                  |
| 2    | yield      | 暂停函数         | yield x          |
| 1    | ,          | 逗号             | 7 , 8            |

**注意：**淡红色指示实验性或建议性的技术（ECMASScript 2016 或 ES7）

**提示：**括号中的表达式会在值在表达式的其余部分中被使用之前进行完全计算。





## JavaScript 数据类型

JavaScript 变量能够保存多种*数据类型*：数值、字符串值、数组、对象等等：

```javascript
var length = 7;                             // 数字
var lastName = "Gates";                      // 字符串
var cars = ["Porsche", "Volvo", "BMW"];         // 数组
var x = {firstName:"Bill", lastName:"Gates"};    // 对象 
```





## JavaScript 数组

JavaScript 数组用方括号书写。

数组的项目由逗号分隔。

下面的代码声明（创建）了名为 cars 的数组，包含三个项目（汽车品牌）：

### 实例

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 数组</h2>

<p>数组索引基于零，这意味着第一个项目是 [0]，第二个项目是 [1]，以此类推。</p>

<p id="demo"></p>

<script>
var cars = ["Porsche", "Volvo", "BMW"];

document.getElementById("demo").innerHTML = cars[0];
</script>

</body>
</html>
```



## JavaScript 对象

JavaScript 对象用花括号来书写。

对象属性是 *name*:*value* 对，由逗号分隔。

### 实例

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
```





## typeof 运算符

您可使用 JavaScript 的 typeof 来确定 JavaScript 变量的类型：

typeof 运算符返回变量或表达式的类型：

### 实例

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript typeof</h2>

<p>typeof 运算符返回变量或表达式的类型：</p>

<p id="demo"></p>
<p id="demo0"></p>

<script>
document.getElementById("demo").innerHTML = 
typeof "" + "<br>" +
typeof "Bill" + "<br>" + 
typeof "Bill Gates";
  
  document.getElementById("demo0").innerHTML = 
typeof 0 + "<br>" + 
typeof 314 + "<br>" +
typeof 3.14 + "<br>" +
typeof (3) + "<br>" +
typeof (3 + 4);
</script>
</body>
</html>
```

###Undefined

在 JavaScript 中，没有值的变量，其值是 undefined。typeof 也返回 undefined。

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 数据类型</h2>

<p>无值变量的值和数据类型是 <b>undefined</b>。</p>

<p id="demo"></p>

<script>
var car;
document.getElementById("demo").innerHTML =
car + "<br>" + typeof car;
</script>

</body>
</html>
```

###空值

空值与 undefined 不是一回事。

空的字符串变量既有值也有类型。

实例

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 数据类型</h2>

<p>空的字符串变量既有值也有类型。</p>

<p id="demo"></p>

<script>
var car = "";
document.getElementById("demo").innerHTML =
"其值是：" +
car + "<br>" +
"类型是：" + typeof car;
</script>

</body>
</html>
```



***\*您也可以通过设置值为 undefined 清空对象：\****

### 实例

```javascript
var person = undefined;     // 值是 undefined，类型是 undefined
```

###Undefined 与 Null 的区别

Undefined 与 null 的值相等，但类型不相等：

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 数据类型</h2>

<p>Undefined and null are equal in value but different in type:</p>

<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML =
typeof undefined + "<br>" +
typeof null + "<br><br>" +
(null === undefined) + "<br>" +
(null == undefined);
</script>

</body>
</html>
```

###原始数据

原始数据值是一种没有额外属性和方法的单一简单数据值。

typeof 运算符可返回以下原始类型之一：

- string
- number
- boolean
- undefined

实例

```javascript
typeof "Bill"              // 返回 "string"
typeof 3.14                // 返回 "number"
typeof true                // 返回 "boolean"
typeof false               // 返回 "boolean"
typeof x                   // 返回 "undefined" (假如 x 没有值)
```

###复杂数据

typeof 运算符可返回以下两种类型之一：

- function
- object

typeof 运算符把对象、数组或 null 返回 object。

typeof 运算符不会把函数返回 object。

实例

```javascript
typeof {name:'Bill', age:62} // 返回 "object"
typeof [1,2,3,4]             // 返回 "object" (并非 "array"，参见下面的注释)
typeof null                  // 返回 "object"
typeof function myFunc(){}   // 返回 "function"
```





# JavaScript 函数



## JavaScript 函数语法

JavaScript 函数通过 function 关键词进行定义，其后是*函数名*和括号 ()。

函数名可包含字母、数字、下划线和美元符号（规则与变量名相同）。

圆括号可包括由逗号分隔的参数：

```
(参数 1, 参数 2, ...)
```

由函数执行的代码被放置在花括号中：*{}*

```
function name(参数 1, 参数 2, 参数 3) {
    要执行的代码
}
```

*函数参数（Function parameters）*是在函数定义中所列的名称。

*函数参数（Function arguments）*是当调用函数时由函数接收的真实的*值*。

在函数中，参数是局部变量。

在其他编程语言中，函数近似程序（Procedure）或子程序（Subroutine）。



## 函数调用

函数中的代码将在其他代码调用该函数时执行：

- 当事件发生时（当用户点击按钮时）
- 当 JavaScript 代码调用时
- 自动的（自调用）



## 函数返回

当 JavaScript 到达 return 语句，函数将停止执行。

如果函数被某条语句调用，JavaScript 将在调用语句之后“返回”执行代码。

函数通常会计算出*返回值*。这个返回值会返回给调用者：

实例

计算两个数的乘积，并返回结果：

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 函数</h2>

<p>本例调用了一个执行计算的函数，然后返回结果：</p>

<p id="demo"></p>

<script>
var x = myFunction(7, 8);
document.getElementById("demo").innerHTML = x;

function myFunction(a, b) {
    return a * b;
}
</script>

</body>
</html>
```

###() 运算符调用函数

使用上面的例子，toCelsius 引用的是函数对象，而 toCelsius() 引用的是函数结果。

实例

访问没有 () 的函数将返回函数定义：

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 函数</h2>

<p>不使用 () 访问函数将返回函数声明而不是函数结果：</p>

<p id="demo"></p>

<script>
function toCelsius(f) {
    return (5/9) * (f-32);
}
document.getElementById("demo").innerHTML = toCelsius;
</script>

</body>
</html>
```



## 局部变量

在 JavaScript 函数中声明的变量，会成为函数的*局部变量*。

局部变量只能在函数内访问。

实例

```javascript
// 此处的代码不能使用 carName

function myFunction() {
    var carName = "Volvo";
    // code here CAN use carName
}

// 此处的代码可以使用 carName
```





#JavaScript 对象

* 您之前已经学到，JavaScript 变量是数据值的容器。

  这段代码把一个*单一值*（porsche）赋给名为 car 的*变量*：

```javascript
var car = "porsche";
```

* 对象也是变量。但是对象包含很多值。

  这段代码把*多个值*（porsche, 911, white）赋给名为 car 的*变量*：

```
var car = {type:"porsche", model:"911", color:"white"};
```

<strong>值以*名称:值*对的方式来书写（名称和值由冒号分隔）。</strong>

<strong>JavaScript 对象是*被命名值*的容器。</strong>





## 对象方法

对象也可以有*方法*。

方法是在对象上执行的*动作*。

方法以*函数定义*被存储在属性中。

| 属性      | 属性值                                                    |
| :-------- | :-------------------------------------------------------- |
| firstName | Bill                                                      |
| lastName  | Gates                                                     |
| age       | 62                                                        |
| eyeColor  | blue                                                      |
| fullName  | function() {return this.firstName + " " + this.lastName;} |

方法是作为属性来存储的函数。

实例

```
var person = {
  firstName: "Bill",
  lastName : "Gates",
  id       : 678,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
```

###this 关键词

在函数定义中，this 引用该函数的“拥有者”。

在上面的例子中，this 指的是“拥有” fullName 函数的 *person 对象*。

换言之，this.firstName 的意思是 *this 对象*的 firstName 属性。

###访问对象属性

您能够以两种方式访问属性：

```javascript
objectName.propertyName
```

或者

```javascript
objectName["propertyName"]
```

###访问对象方法

您能够通过如下语法访问对象方法：

```javascript
objectName.methodName()
```

实例

```javascript
name = person.fullName();
```

如果您*不使用 ()* 访问 fullName 方法，则将返回*函数定义*：

实例

```javascript
name = person.fullName;
```



## 请不要把字符串、数值和布尔值声明为对象！

如果通过关键词 "new" 来声明 JavaScript 变量，则该变量会被创建为对象：

```
var x = new String();        // 把 x 声明为 String 对象
var y = new Number();        // 把 y 声明为 Number 对象
var z = new Boolean();       //	把 z 声明为 Boolean 对象
```

请避免字符串、数值或逻辑对象。他们会增加代码的复杂性并降低执行速度。



# JavaScript 事件

## HTML 事件

HTML 事件可以是浏览器或用户做的某些事情。

下面是 HTML 事件的一些例子：

- HTML 网页完成加载
- HTML 输入字段被修改
- HTML 按钮被点击

通常，当事件发生时，用户会希望做某件事。

JavaScript 允许您在事件被侦测到时执行代码。

*通过 JavaScript 代码*，HTML 允许您向 HTML 元素添加事件处理程序。

使用单引号：

```html
<element event='一些 JavaScript'>
```

使用双引号：

```html
<element event="一些 JavaScript">
```

## 常见的 HTML 事件

下面是一些常见的 HTML 事件：

| 事件        | 描述                         |
| :---------- | :--------------------------- |
| onchange    | HTML 元素已被改变            |
| onclick     | 用户点击了 HTML 元素         |
| onmouseover | 用户把鼠标移动到 HTML 元素上 |
| onmouseout  | 用户把鼠标移开 HTML 元素     |
| onkeydown   | 用户按下键盘按键             |
| onload      | 浏览器已经完成页面加载       |



## JavaScript 能够做什么？

事件处理程序可用于处理、验证用户输入、用户动作和浏览器动作：

- 每当页面加载时应该做的事情
- 当页面被关闭时应该做的事情
- 当用户点击按钮时应该被执行的动作
- 当用户输入数据时应该被验证的内容
- 等等

让 JavaScript 处理事件的不同方法有很多：

- HTML 事件属性可执行 JavaScript 代码
- HTML 事件属性能够调用 JavaScript 函数
- 您能够向 HTML 元素分配自己的事件处理函数
- 您能够阻止事件被发送或被处理





# JavaScript 字符串

**JavaScript 字符串用于存储和操作文本。**

JavaScript 字符串是引号中的零个或多个字符。

实例

```javascript
var x = "Bill Gates"
```

##字符串长度

内建属性 length 可返回字符串的*长度*：

实例

```javascript
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
var sln = txt.length;
```

## 特殊字符

由于字符串必须由引号包围，JavaScript 会误解这段字符串：

```
var y = "中国是瓷器的故乡，因此 china 与"China（中国）"同名。"
```

该字符串将被切为 "中国是瓷器的故乡，因此 china 与"。

避免此问题的解决方法是，使用 *\ 转义字符*。

反斜杠转义字符把特殊字符转换为字符串字符：

| 代码 | 结果 | 描述   |
| :--- | :--- | :----- |
| \'   | '    | 单引号 |
| \"   | "    | 双引号 |
| \\   | \    | 反斜杠 |

实例

序列 \" 在字符串中插入双引号：

实例

```javascript
var x = "中国是瓷器的故乡，因此 china 与\"China（中国）\"同名。"
```

转义字符（\）也可用于在字符串中插入其他特殊字符。

其他六个 JavaScript 中有效的转义序列：

| 代码 | 结果       |
| :--- | :--------- |
| \b   | 退格键     |
| \f   | 换页       |
| \n   | 新行       |
| \r   | 回车       |
| \t   | 水平制表符 |
| \v   | 垂直制表符 |

这六个转义字符最初设计用于控制打字机、电传打字机和传真机。它们在 HTML 中没有任何意义。



```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 语句</h2>

<p>
折行的最佳位置是运算符或逗号之后。
</p>

<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML =
"Hello Kitty.";
</script>

</body>
</html>
```

## 字符串可以是对象

请不要把字符串创建为对象。它会拖慢执行速度。

new 关键字使代码复杂化。也可能产生一些意想不到的结果：

当使用 == 相等运算符时，相等字符串是相等的：

实例

```
var x = "Bill";             
var y = new String("Bill");

// (x == y) 为 true，因为 x 和 y 的值相等
```

```html
<!DOCTYPE html>
<html>
<body>

<h1>绝不要把字符串创建为对象</h1>

<p>字符串与对象无法安全地比较。</p>

<p id="demo"></p>

<script>
var x = "Bill";              // x 是字符串
var y = new String("Bill");  // y 是对象
document.getElementById("demo").innerHTML = (x==y);
</script>

</body>
</html>
```



当使用 === 运算符时，相等字符串是不相等的，因为 === 运算符需要类型和值同时相等。

实例

```
var x = "Bill";             
var y = new String("Bill");

// (x === y) 为 false，因为 x 和 y 的类型不同（字符串与对象）
```

```html
<!DOCTYPE html>
<html>
<body>

<h1>绝不要把字符串创建为对象</h1>

<p>JavaScript 对象无法比较。</p>

<p id="demo"></p>

<script>
var x = "Bill";              // x 是字符串
var y = new String("Bill");  // y 是对象
document.getElementById("demo").innerHTML = (x===y);
</script>

</body>
</html>
```



甚至更糟。对象无法比较：

```html
<!DOCTYPE html>
<html>
<body>

<h1>绝不要把字符串创建为对象</h1>

<p>JavaScript 对象无法比较。</p>

<p id="demo"></p>

<script>
var x = new String("Bill");  // x 是对象
var y = new String("Bill");  // y 也是对象
document.getElementById("demo").innerHTML = (x==y);
</script>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<body>

<h1>绝不要把字符串创建为对象</h1>

<p>JavaScript 对象无法比较。</p>

<p id="demo"></p>

<script>
var x = new String("Bill");  // x 是对象
var y = new String("Bill");  // y 也是对象
document.getElementById("demo").innerHTML = (x===y);
</script>

</body>
</html>
```

***PS:请注意 (x==y) 与 (x===y) 的区别。***

JavaScript 对象无法进行对比，比较两个 JavaScript 将始终返回 false。



##字符串的方法

###查找字符串中的字符串

indexOf() 方法返回字符串中指定文本*首次*出现的索引（位置）：

实例

```javascript
var str = "The full name of China is the People's Republic of China.";
var pos = str.indexOf("China");
```

JavaScript 从零计算位置。

0 是字符串中的第一个位置，1 是第二个，2 是第三个 ...

lastIndexOf() 方法返回指定文本在字符串中*最后*一次出现的索引：

实例

```javascript
var str = "The full name of China is the People's Republic of China.";
var pos = str.lastIndexOf("China");
```

如果未找到文本， indexOf() 和 lastIndexOf() 均返回 -1。



***两种方法都接受作为检索起始位置的第二个参数。***

实例

```javascript
var str = "The full name of China is the People's Republic of China.";
var pos = str.indexOf("China", 18);
```



lastIndexOf() 方法向后进行检索（从尾到头），这意味着：假如第二个参数是 50，则从位置 50 开始检索，直到字符串的起点。

实例

```javascript
var str = "The full name of China is the People's Republic of China.";
var pos = str.lastIndexOf("China", 50);
```



###检索字符串中的字符串

search() 方法搜索特定值的字符串，并返回匹配的位置：

实例

```javascript
var str = "The full name of China is the People's Republic of China.";
var pos = str.search("locate");
```





***两种方法，indexOf() 与 search()，是*相等的*。***

这两种方法是不相等的。区别在于：

- search() 方法无法设置第二个开始位置参数。
- indexOf() 方法无法设置更强大的搜索值（正则表达式）。



###提取部分字符串

有三种提取部分字符串的方法：

- slice(*start*, *end*)
- substring(*start*, *end*)
- substr(*start*, *length*)

####slice() 方法

slice() 提取字符串的某个部分并在新字符串中返回被提取的部分。

该方法设置两个参数：起始索引（开始位置），终止索引（结束位置）。

这个例子裁剪字符串中位置 7 到位置 13 的片段：

实例

```javascript
var str = "Apple, Banana, Mango";
var res = str.slice(7,13);
```

res 的结果是：

```
Banana
```



如果某个参数为负，则从字符串的结尾开始计数。

这个例子裁剪字符串中位置 -12 到位置 -6 的片段：

实例

```javascript
var str = "Apple, Banana, Mango";
var res = str.slice(-13,-7);
```

res 的结果是：

```
Banana
```



如果省略第二个参数，则该方法将裁剪字符串的剩余部分：

实例

```javascript
var res = str.slice(7);
```

或者从结尾计数：

实例

```javascript
var res = str.slice(-13);
```



####substring() 方法

substring() 类似于 slice()。

不同之处在于 substring() 无法接受负的索引。

实例

```javascript
var str = "Apple, Banana, Mango";
var res = str.substring(7,13);
```

res 的结果是：

```
Banana
```

如果省略第二个参数，则该 substring() 将裁剪字符串的剩余部分。



####substr() 方法

substr() 类似于 slice()。

不同之处在于第二个参数规定被提取部分的*长度*。

实例

```javascript
var str = "Apple, Banana, Mango";
var res = str.substr(7,6);
```

res 的结果是：

```
Banana
```

如果省略第二个参数，则该 substr() 将裁剪字符串的剩余部分。



如果首个参数为负，则从字符串的结尾计算位置。

实例

```javascript
var str = "Apple, Banana, Mango";
var res = str.substr(-5);
```





###替换字符串内容

replace() 方法用另一个值替换在字符串中指定的值：

实例

```javascript
str = "Please visit Microsoft!";
var n = str.replace("Microsoft", "W3School");
```

replace() 方法不会改变调用它的字符串。它返回的是新字符串。

默认地，replace() *只替换首个匹配*：

实例

```javascript
str = "Please visit Microsoft and Microsoft!";
var n = str.replace("Microsoft", "W3School");
```

默认地，replace() 对大小写敏感。因此不对匹配 MICROSOFT：

实例

```javascript
str = "Please visit Microsoft!";
var n = str.replace("MICROSOFT", "W3School");
```

如需执行大小写不敏感的替换，请使用正则表达式 /i（大小写不敏感）：

实例

```javascript
str = "Please visit Microsoft!";
var n = str.replace(/MICROSOFT/i, "W3School");
```

请注意正则表达式不带引号。

如需替换所有匹配，请使用正则表达式的 g 标志（用于全局搜索）：

实例

```javascript
str = "Please visit Microsoft and Microsoft!";
var n = str.replace(/Microsoft/g, "W3School");
```



###转换为大写和小写

通过 toUpperCase() 把字符串转换为大写：

实例

```javascript
var text1 = "Hello World!";       // 字符串
var text2 = text1.toUpperCase();  // text2 是被转换为大写的 text1
```

通过 toLowerCase() 把字符串转换为小写：

实例

```javascript
var text1 = "Hello World!";       // 字符串
var text2 = text1.toLowerCase();  // text2 是被转换为小写的 text1
```



###concat() 方法

concat() 连接两个或多个字符串：

实例

```javascript
var text1 = "Hello";
var text2 = "World";
text3 = text1.concat(" ",text2);
```

concat() 方法可用于代替加运算符。下面两行是等效的：

```javascript
var text = "Hello" + " " + "World!";
var text = "Hello".concat(" ","World!");
```



###String.trim()

trim() 方法删除字符串两端的空白符：

实例：

```javascript
var str = "       Hello World!        ";
alert(str.trim());
```

您可搭配正则表达式使用 replace() 方法代替：

```javascript
var str = "       Hello World!        ";
alert(str.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, ''));
```





###提取字符串字符

这是两个提取字符串字符的*安全*方法：

- charAt(*position*)
- charCodeAt(*position*)

####charAt() 方法

charAt() 方法返回字符串中指定下标（位置）的字符串：

实例

```javascript
var str = "HELLO WORLD";
str.charAt(0);            // 返回 H
```

####charCodeAt() 方法

charCodeAt() 方法返回字符串中指定索引的字符 unicode 编码：

实例

```javascript
var str = "HELLO WORLD";

str.charCodeAt(0);         // 返回 72
```



###属性访问（Property Access）

ECMAScript 5 (2009) 允许对字符串的属性访问 [ ]：

实例

```javascript
var str = "HELLO WORLD";
str[0];                   // 返回 H
```

使用属性访问有点不太靠谱：

- 不适用 Internet Explorer 7 或更早的版本
- 它让字符串看起来像是数组（其实并不是）
- 如果找不到字符，[ ] 返回 undefined，而 charAt() 返回空字符串。
- 它是只读的。str[0] = "A" 不会产生错误（但也不会工作！）



###把字符串转换为数组

可以通过 split() 将字符串转换为数组：

实例

```javascript
var txt = "a,b,c,d,e";   // 字符串
txt.split(",");          // 用逗号分隔
txt.split(" ");          // 用空格分隔
txt.split("|");          // 用竖线分隔
```

如果省略分隔符，被返回的数组将包含 index [0] 中的整个字符串。

如果分隔符是 ""，被返回的数组将是间隔单个字符的数组：





## JavaScript 数值

* 始终是 64 位的浮点数

与许多其他编程语言不同，JavaScript 不会定义不同类型的数，比如整数、短的、长的、浮点的等等。

JavaScript 数值始终以双精度浮点数来存储，根据国际 IEEE 754 标准。

此格式用 64 位存储数值，其中 0 到 51 存储数字（片段），52 到 62 存储指数，63 位存储符号：

| 值(aka Fraction/Mantissa) | 指数              | 符号       |
| :------------------------ | :---------------- | :--------- |
| 52 bits(0 - 51)           | 11 bits (52 - 62) | 1 bit (63) |

###精度

整数（不使用指数或科学计数法）会被精确到 15 位。

实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数字</h1>

<p>整数（不使用指数或科学计数法）会被精确到 15 位：</p>

<p id="demo"></p>

<script>
var x = 999999999999999;
var y = 9999999999999999;
document.getElementById("demo").innerHTML = x + "<br>" + y;
</script>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数字</h1>

<p>浮点的算数并不总是 100% 精准：</p>

<p id="demo"></p>

<script>
var x = 0.2 + 0.1;
document.getElementById("demo").innerHTML = "0.2 + 0.1 = " + x;
</script>

</body>
</html>

```

使用乘除法有助于解决上面的问题：

实例

```javascript
var x = (0.2 * 10 + 0.1 * 10) / 10;       // x 将是 0.3
```



###数字和字符串相加

* 警告！！

JavaScript 的加法和级联（concatenation）都使用 + 运算符。

数字用加法。字符串用级联。

如果您对两个数相加，结果将是一个数：

实例

```javascript
var x = 10;
var y = 20;
var z = x + y;           // z 将是 30（一个数）
```

如果对两个字符串相加，结果将是一个字符串的级联：

实例

```javascript
var x = "10";
var y = "20";
var z = x + y;           // z 将是 1020（字符串）
```



###数字字符串

JavaScript 字符串可以拥有数字内容：

```javascript
var x = 100;         // x 是数字

var y = "100";       // y 是字符串
```

在所有数字运算中，JavaScript 会尝试将字符串转换为数字：

该例如此运行：

```javascript
var x = "100";
var y = "10";
var z = x / y;       // z 将是 10
```



###NaN - 非数值

NaN 属于 JavaScript 保留词，指示某个数不是合法数。

尝试用一个非数字字符串进行除法会得到 NaN（Not a Number）：

实例

```javascript
var x = 100 / "Apple";  // x 将是 NaN（Not a Number）
```

您可使用全局 JavaScript 函数 isNaN() 来确定某个值是否是数：

实例

```javascript
var x = 100 / "Apple";
isNaN(x);               // 返回 true，因为 x 不是数
```

要小心 NaN。假如您在数学运算中使用了 NaN，则结果也将是 NaN：

实例

```javascript
var x = NaN;
var y = 5;
var z = x + y;         // z 将是 NaN
```

NaN 是数，typeof NaN 返回 number：

实例

```javascript
typeof NaN;             // 返回 "number"
```



###Infinity

Infinity （或 -Infinity）是 JavaScript 在计算数时超出最大可能数范围时返回的值。

实例

```javascript
var myNumber = 2;

while (myNumber != Infinity) {          // 执行直到 Infinity
    myNumber = myNumber * myNumber;
}
```

除以 0（零）也会生成 Infinity：

实例

```javascript
var x =  2 / 0;          // x 将是 Infinity
var y = -2 / 0;          // y 将是 -Infinity
```



###十六进制

JavaScript 会把前缀为 0x 的数值常量解释为十六进制。

实例

```javascript
var x = 0xFF;             // x 将是 255
```

绝不要用前导零写数字（比如 07）。

一些 JavaScript 版本会把带有前导零的数解释为八进制。

默认地，Javascript 把数显示为十进制小数。

但是您能够使用 toString() 方法把数输出为十六进制、八进制或二进制。

实例

```javascript
var myNumber = 128;
myNumber.toString(16);     // 返回 80
myNumber.toString(8);      // 返回 200
myNumber.toString(2);      // 返回 10000000
```



###数值可以是对象

通常 JavaScript 数值是通过字面量创建的原始值：var x = 123

但是也可以通过关键词 new 定义为对象：var y = new Number(123)

实例

```javascript
var x = 123;
var y = new Number(123);

// typeof x 返回 number
// typeof y 返回 object
```



请不要创建数值对象。这样会拖慢执行速度。

new 关键词使代码复杂化，并产生某些无法预料的结果：

当使用 == 相等运算符时，相等的数看上去相等：

实例

```javascript
var x = 500;             
var y = new Number(500);

// (x == y) 为 true，因为 x 和 y 有相等的值
```



当使用 === 相等运算符后，相等的数变为不相等，因为 === 运算符需要类型和值同时相等。

实例

```javascript
var x = 500;             
var y = new Number(500);

// (x === y) 为 false，因为 x 和 y 的类型不同
```



甚至更糟。对象无法进行对比：

实例

```javascript
var x = new Number(500);             
var y = new Number(500);

// (x == y) 为 false，因为对象无法比较
```

JavaScript 对象无法进行比较。



###数值方法

* toString() 

* toExponential()

  ```javascript
  var x = 9.656;
  x.toExponential(2);     // 返回 9.66e+0
  x.toExponential(4);     // 返回 9.6560e+0
  x.toExponential(6);     // 返回 9.656000e+0
  ```

* toFixed() 返回字符串值，它包含了指定位数小数的数字：

  ```javascript
  var x = 9.656;
  x.toFixed(0);           // 返回 10
  x.toFixed(2);           // 返回 9.66
  x.toFixed(4);           // 返回 9.6560
  x.toFixed(6);           // 返回 9.656000
  ```

* toPrecision() 返回字符串值，它包含了指定长度的数字：

  ```javascript
  var x = 9.656;
  x.toPrecision();        // 返回 9.656
  x.toPrecision(2);       // 返回 9.7
  x.toPrecision(4);       // 返回 9.656
  x.toPrecision(6);       // 返回 9.65600
  ```

* valueOf() 以数值返回数值：

  ```javascript
  var x = 123;
  x.valueOf();            // 从变量 x 返回 123
  (123).valueOf();        // 从文本 123 返回 123
  (100 + 23).valueOf();   // 从表达式 100 + 23 返回 123
  ```

在 JavaScript 中，数字可以是原始值（typeof = number）或对象（typeof = object）。

在 JavaScript 内部使用 valueOf() 方法可将 Number 对象转换为原始值。

没有理由在代码中使用它。

所有 JavaScript 数据类型都有 valueOf() 和 toString() 方法。



###把变量转换为数值

####全局方法

JavaScript 全局方法可用于所有 JavaScript 数据类型。

这些是在处理数字时最相关的方法：

| 方法         | 描述                         |
| :----------- | :--------------------------- |
| Number()     | 返回数字，由其参数转换而来。 |
| parseFloat() | 解析其参数并返回浮点数。     |
| parseInt()   | 解析其参数并返回整数。       |



####用于日期的 Number() 方法

Number() 还可以把日期转换为数字：

实例

```javascript
Number(new Date("2019-04-15"));    // 返回 1506729600000
```



###数值属性

| 属性              | 描述                             |
| :---------------- | :------------------------------- |
| MAX_VALUE         | 返回 JavaScript 中可能的最大数。 |
| MIN_VALUE         | 返回 JavaScript 中可能的最小数。 |
| NEGATIVE_INFINITY | 表示负的无穷大（溢出返回）。     |
| NaN               | 表示非数字值（"Not-a-Number"）。 |
| POSITIVE_INFINITY | 表示无穷大（溢出返回）。         |
