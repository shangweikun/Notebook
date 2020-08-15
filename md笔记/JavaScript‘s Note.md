# Begin JavaScrip

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

