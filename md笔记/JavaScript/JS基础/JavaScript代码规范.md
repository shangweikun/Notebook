## 变量名

在 W3School，我们对标识符名称（变量和函数）使用了*驼峰式大小写*。

所有名称以*字母*开头。

在本页的底部，我们会更广泛地讨论命名规则。

```javascript
firstName = "Bill";
lastName = "Gates";

price = 19.90;
tax = 0.20;

fullPrice = price + (price * tax);
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

### 函数

```javascript
function toCelsius(fahrenheit) {
    return (5 / 9) * (fahrenheit - 32);
}
```

请不要对缩进使用制表符。不同的编辑器对 tab 的解释也不尽相同。

## 语句规则

简单语句的通用规则：

请始终以分号结束单条语句：

### 实例

```javascript
var values = ["Volvo", "Saab",  "Fiat"];

var person = {
    firstName: "Bill",
     lastName: "Gates",
    age: 50,
    eyeColor:  "blue"
};
```

针对复杂语句（compound）的通用规则：

- 请在第一行的结尾处写开括号
- 请在开括号前使用一个空格
- 请在新行上写闭括号，不带前导空格
- 请不要以分号来结束复杂语句

### 函数：

```javascript
function toCelsius(fahrenheit) {
    return (5 / 9) * (fahrenheit - 32);
}
```

### 循环：

```javascript
for (i = 0; i < 5; i++) {
    x += i;
}
```

### 条件：

```javascript
if (time < 20) {
    greeting = "Good day";
} else {
     greeting = "Good evening";
}
```

## 对象规则

针对对象定义的通用规则：

- 把开括号与对象名放在同一行
- 在每个属性与其值之间使用冒号加一个空格
- 不要在最后一个属性值对后面写逗号
- 请在新行上写闭括号，不带前导空格
- 请始终以分号结束对象定义

### 实例

```javascript
var person = {
    firstName: "Bill",
    lastName: "Gates",
    age: 50,
    eyeColor:  "blue"
};
```

可以对短对象在一行中进行压缩，只在属性之间使用空格，就像这样：

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:50, eyeColor:"blue"};
```



## 行长度小于 80

为了提高可读性，请避免每行的长度超过 80 个字符。

如果 JavaScript 语句超过一行的长度，换行的最佳位置是运算符或逗号之后。

### 实例

```javascript
document.getElementById("demo").innerHTML =
    "Hello Kitty."; 
```



## 命名约定

请始终对您所有的代码使用相同的命名约定。例如：

- 变量和函数名以*驼峰大小写*来写
- 全局变量使用*大写*（我们不这样做，但是相当普遍）
- 常量（比如 PI）使用*大写*

我们是否应在变量名中使用*连字符*、*驼峰大小写*或*下划线*吗？

这是程序员们经常讨论的问题。答案取决于这个问题是谁回答的：



### HTML 和 CSS 中的连字符：

HTML5 属性能够以 data- 开头（data-quantity, data-price）。

CSS 在 property-names 中使用连字符（font-size）。

Hyphens 可被错误地视为减法运算符。JavaScript 命名不允许使用连字符。

### 下划线：

许多程序员喜欢使用下划线（date_of_birth），特别是在 SQL 数据库中。

下划线经常被用在 PHP 参考资料中。

### 帕斯卡命名法（PascalCase）：

C 语言程序员经常使用帕斯卡命名法。

### 驼峰大小写（camelCase）：

JavaScript 本身、jQuery 以及其他 JavaScript 库使用驼峰大小写。

JavaScript 命名请不要以 $ 符号开头。此举会引起 JavaScript 库名称冲突。



## 在 HTML 中加载 JavaScript

使用简单的语法来加载外部脚本（type 属性不是必需的）：

```html
<script src="myscript.js"></script>
```



## 访问 HTML 元素

使用“不整洁的” HTML 样式的后果，也许是导致 JavaScript 错误。

这两条 JavaScript 语句会产生不同的结果：

```
var obj = getElementById("Demo")

var obj = getElementById("demo") 
```

如果可能，请在 HTML 中使用相同的命名约定（就像 JavaScript 那样）。



## 文件扩展名

HTML 文件应该使用 *.html* 扩展名（而非 *.htm*）。

CSS 文件应该使用 *.css* 扩展名。

JavaScript 文件应该使用 *.js* 扩展名。

## 使用小写文件名

大多数 web 服务器（Apache、Unix）对文件名的大小写敏感：

london.jpg 无法视作 London.jpg 进行访问。

其他 web 服务器（微软的 IIS）对大小写不敏感：

london.jpg 能够以 London.jpg 或 london.jpg 来访问。

如果您混合使用大小写，则必须严格保持连续和一致。

如果您将站点从大小写不敏感的服务器转移至对大小写敏感的服务器，即使这种小错误也可能破坏您的网站。

为了避免这些问题，请始终使用小写文件名（如果可能）。

## 性能

计算机不会使用代码约定。大部分规则对程序的执行影响很小。

缩进和额外的空格对小段脚本并不重要。

对于开发中的脚本，应该优先考虑可读性。应该缩小更大型的生产脚本。



# JavaScript 最佳实践



**请避免全局变量、new、===、eval()**



## 避免全局变量

请尽量少地使用全局变量。

它包括所有的数据类型、对象和函数。

全局变量和函数可被其他脚本覆盖。

请使用局部变量替代，并学习如何使用[闭包](https://www.w3school.com.cn/js/js_function_closures.asp)。



## 始终声明局部变量

所有在函数中使用的变量应该被声明为*局部*变量。

局部变量*必须*通过 *var* 关键词来声明，否则它们将变成全局变量。

严格模式不允许未声明的变量。



## 在顶部声明

一项好的编码习惯是把所有声明放在每段脚本或函数的顶部。

这么做的好处是：

- 获得更整洁的代码
- 提供了查找局部变量的好位置
- 更容易避免不需要的全局变量
- 减少不需要的重新声明的可能性



```javascript
// 在顶部声明
var firstName, lastName, price, discount, fullPrice;

// 稍后使用
firstName = "Bill";
lastName = "Gates";

price = 19.90;
discount = 0.10;

fullPrice = price * 100 / discount;
```



也可以用于循环变量：

```javascript
// 在顶部声明
var i;

// 稍后使用
for (i = 0; i < 5; i++)  {
```

默认地，JavaScript 会将所有声明移至顶部（JavaScript hoisting）。



## 初始化变量

在您声明变量时对其进行初始化是个好习惯。

这么做的好处是：

- 更整洁的代码
- 在单独的位置来初始化变量
- 避免未定义值

```javascript
// 在开头进行声明和初始化
var firstName = "",
    lastName  = "",
    price = 0,
    discount = 0,
    fullPrice  = 0,
    myArray = [],
    myObject = {};
```

变量初始化使我们能够了解预期用途和预期的数据类型。





## 请不要声明数值、字符串或布尔对象

请始终将数值、字符串或布尔值视作原始值。而非对象。

如果把这些类型声明为对象，会拖慢执行速度，并产生讨厌的副作用：

### 实例

```javascript
var x = "Bill";             
var y = new String("Bill");
(x === y) // 结果为 false，因为 x 是字符串，而 y 是对象。
```



或者甚至更糟：

### 实例

```javascript
var x = new String("Bill");             
var y = new String("Bill");
(x == y) // 结果是 false，因为你无法比较对象。
```





## 请勿使用 new Object()

- 请使用 {} 来代替 new Object()
- 请使用 "" 来代替 new String()
- 请使用 0 来代替 new Number()
- 请使用 false 来代替 new Boolean()
- 请使用 [] 来代替 new Array()
- 请使用 /()/ 来代替 new RegExp()
- 请使用 function (){}来代替 new Function()

### 实例

```javascript
var x1 = {};           // 新对象
var x2 = "";           // 新的原始字符串值
var x3 = 0;            // 新的原始数值
var x4 = false;        // 新的原始布尔值
var x5 = [];           // 新的数组对象
var x6 = /()/;         // 新的正则表达式
var x7 = function(){}; // 新的函数对象
```



## 意识到自动类型转换

请意识到数值会被意外转换为字符串或 NaN（Not a Number）。

JavaScript 属于松散类型。变量可包含不同的数据类型，并且变量能够改变其数据类型：

### 实例

```javascript
var x = "Hello";     // typeof x 为字符串
x = 5;               // 把 typeof x 更改为数值
```



```javascript
var x = 5 + 7;       // x.valueOf() 是 12,  typeof x 是数值
var x = 5 + "7";     // x.valueOf() 是 57,  typeof x 是字符串
var x = "5" + 7;     // x.valueOf() 是 57,  typeof x 是字符串
var x = 5 - 7;       // x.valueOf() 是 -2,  typeof x 是数值
var x = 5 - "7";     // x.valueOf() 是 -2,  typeof x 是数值
var x = "5" - 7;     // x.valueOf() 是 -2,  typeof x 是数值
var x = 5 - "x";     // x.valueOf() 是 NaN, typeof x 是数值
```



用字符串减去字符串，不会产生错误而是返回 NaN（Not a Number）：

### 实例

```javascript
"Hello" - "Dolly"    // 返回 NaN
```



## 使用 === 比较

== 比较运算符总是在比较之前进行类型转换（以匹配类型）。

=== 运算符会强制对值和类型进行比较：

### 实例

```javascript
0 == "";        // true
1 == "1";       // true
1 == true;      // true

0 === "";       // false
1 === "1";      // false
1 === true;     // false
```



## 使用 Parameter Defaults

如果调用函数时缺少一个参数，那么这个缺失参数的值会被设置为 undefined。

undefined 值会破坏您的代码。为参数设置默认值是一个好习惯。

### 实例

```javascript
function myFunction(x, y) {
    if (y === undefined) {
        y = 0;
    }
}
```





## 用 default 来结束 switch

请使用使用 default 来结束您的 switch 语句。即使您认为没有这个必要。

### 实例

```javascript
switch (new Date().getDay()) {
    case 0:
        day = "Sunday";
        break;
    case 1:
        day = "Monday";
         break;
    case 2:
        day = "Tuesday";
         break;
    case 3:
        day = "Wednesday";
         break;
    case 4:
        day = "Thursday";
         break;
    case 5:
        day = "Friday";
         break;
    case 6:
        day = "Saturday";
         break;
    default:
        day =  "Unknown";
} 
```



## 避免使用 eval()

eval() 函数用于将文本作为代码来允许。在几乎所有情况下，都没有必要使用它。

因为允许任意代码运行，它同时也意味着安全问题。