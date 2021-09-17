# JavaScript 错误

```javascript
<p id="demo"></p>

<script>
try {
    adddlert("欢迎访问！");
}
catch(err) {
    document.getElementById("demo").innerHTML = err.message;
}
</script>
```



## JavaScript 抛出错误

当发生错误时，JavaScript 通常会停止并产生错误消息。

技术术语是这样描述的：*JavaScript 将抛出异常（抛出错误）*。

JavaScript 实际上会创建带有两个属性的 *Error 对象*：name 和 message。



## Error 对象

JavaScript 拥有当错误发生时提供错误信息的内置 error 对象。

error 对象提供两个有用的属性：name 和 message。

## Error 对象属性

| 属性    | 描述                             |
| :------ | :------------------------------- |
| name    | 设置或返回错误名                 |
| message | 设置或返回错误消息（一条字符串） |



## Error Name Values

error 的 name 属性可返回六个不同的值：

| 错误名         | 描述                          |
| :------------- | :---------------------------- |
| EvalError      | 已在 eval() 函数中发生的错误  |
| RangeError     | 已发生超出数字范围的错误      |
| ReferenceError | 已发生非法引用                |
| SyntaxError    | 已发生语法错误                |
| TypeError      | 已发生类型错误                |
| URIError       | 在 encodeURI() 中已发生的错误 |



[JavaScript 错误]https://www.w3school.com.cn/js/js_errors.asp



# JavaScript 作用域



## JavaScript 函数作用域

在 JavaScript 中有两种作用域类型：

- 局部作用域
- 全局作用域

JavaScript 拥有函数作用域：每个函数创建一个新的作用域。

作用域决定了这些变量的可访问性（可见性）。

函数内部定义的变量从函数外部是不可访问的（不可见的）。



## 全局 JavaScript 变量

函数之外声明的变量，会成为*全局变量*。

全局变量的作用域是*全局的*：网页的所有脚本和函数都能够访问它。

### 实例

```javascript
var carName = " porsche";


// 此处的代码能够使用 carName 变量

function myFunction() {

    // 此处的代码也能够使用 carName 变量

}
```

## 自动全局

如果您为尚未声明的变量赋值，此变量会自动成为*全局*变量。

这段代码将声明一个全局变量 carName，即使在函数内进行了赋值。

### 实例

```javascript
myFunction();


// 此处的代码能够使用 carName 变量

function myFunction() {
    carName = "porsche";
}
```



# JavaScript 提升（Hoisting）

**提升（Hoisting）是 JavaScript 将声明移至顶部的默认行为。**



## JavaScript 声明会被提升

在 JavaScript 中，可以在使用变量之后对其进行声明。

换句话说，可以在声明变量之前使用它。

例子 1 与例子 2 的结果相同：

### 例子 1

```javascript
x = 5; // 把 5 赋值给 x
 
elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x;                     // 在元素中显示 x

var x; // 声明 x
```

### 例子 2

```javascript
var x; // 声明 x
x = 5; // 把 5 赋值给 x

elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x;                     // 在元素中显示 x
```



## let 和 const 关键字

用 let 或 const 声明的变量和常量不会被提升！





# JavaScript Use Strict



## 声明严格模式

通过在脚本或函数的开头添加 "use strict"; 来声明严格模式。

在脚本开头进行声明，拥有全局作用域（脚本中的所有代码均以严格模式来执行）：

实例

```javascript
"use strict";
x = 3.14;       // 这会引发错误，因为 x 尚未声明
```

在函数中声明严格模式，拥有局部作用域（只有函数中的代码以严格模式执行）：

```javascript
x = 3.14;       // 这不会引发错误
myFunction();

function  myFunction() {
	"use strict";
	 y = 3.14;   // 这会引发错误
}
```



## 严格模式中不允许的事项

在不声明变量的情况下使用变量，是不允许的：

```javascript
"use strict";
x = 3.14;                // 这将引发错误
```



对象也是变量

在不声明对象的情况下使用对象也是不允许的：

```javascript
"use strict";
x = {p1:10, p2:20};      // 这将引发错误
```



删除变量（或对象）是不允许的：

```javascript
"use strict";
var x = 3.14;
delete x;                // 这将引发错误
```



删除函数是不允许的：

```javascript
"use strict";
function x(p1, p2) {}; 
delete x;                 // 这将引发错误
```



重复参数名是不允许的：

```javascript
"use strict";
function x(p1, p1) {};   // 这将引发错误
```



八进制数值文本是不允许的：

```javascript
"use strict";
var x = 010;             // 这将引发错误
```



转义字符是不允许的：

```javascript
"use strict";
var x = \010;            // 这将引发错误
```



写入只读属性是不允许的：

```javascript
"use strict";
var obj = {};
Object.defineProperty(obj, "x", {value:0, writable:false});

obj.x = 3.14;            // 这将引发错误
```



写入只能获取的属性是不允许的：

```javascript
"use strict";
var obj = {get x() {return 0} };

obj.x = 3.14;            // 这将引发错误
```



删除不可删除的属性是不允许的：

```javascript
"use strict";
delete Object.prototype; // 这将引发错误
```



字符串 "eval" 不可用作变量：

https://www.w3school.com.cn/jsref/jsref_eval.asp

```javascript
"use strict";
var eval = 3.14;         // 这将引发错误
```



字符串 "arguments" 不可用作变量：

```javascript
"use strict";
var arguments = 3.14;    // 这将引发错误
```



with 语句是不允许的：

https://www.w3school.com.cn/js/pro_js_statements_with.asp

```javascript
"use strict";
with (Math){x = cos(2)}; // 这将引发错误
```



处于安全考虑，不允许 eval() 在其被调用的作用域中创建变量：

```javascript
"use strict";
eval ("var x = 2");
alert (x);               // 这将引发错误
```



严格模式中不允许使用为未来预留的关键词。它们是：

- implements
- interface
- let
- package
- private
- protected
- public
- static
- yield



## 警告

"use strict" 指令只能在脚本或函数的*开头*被识别。



# JavaScript this 关键词

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  id       : 678,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
```

## this 是什么？

JavaScript this 关键词指的是它所属的对象。

它拥有不同的值，具体取决于它的使用位置：

- 在方法中，this 指的是所有者对象。
- 单独的情况下，this 指的是全局对象。
- 在函数中，this 指的是全局对象。
- 在函数中，严格模式下，this 是 undefined。
- 在事件中，this 指的是接收事件的元素。

像 call() 和 apply() 这样的方法可以将 this 引用到任何对象。



## 单独的 this

在单独使用时，拥有者是全局对象，因此 this 指的是全局对象。

在浏览器窗口中，全局对象是 [object Window]：

实例

```javascript
var x = this;
```



在严格模式中，如果单独使用，那么 this 指的是全局对象 [object Window]：

实例

```javascript
"use strict";
var x = this;
```



## 函数中的 this（默认）

在 JavaScript 函数中，函数的拥有者默认绑定 this。

因此，在函数中，this 指的是全局对象 [object Window]。

实例

```javascript
function myFunction() {
  return this;
}
```



## 函数中的 this（严格模式）

JavaScript 严格模式不允许默认绑定。

因此，在函数中使用时，在严格模式下，this 是未定义的（undefined）。

实例

```javascript
"use strict";
function myFunction() {
  return this;
}
```



## 事件处理程序中的 this

在 HTML 事件处理程序中，this 指的是接收此事件的 HTML 元素：

### 实例

```html
<button onclick="this.style.display='none'">
  点击来删除我！
</button>
#button这个元素
```



## 显式函数绑定

call() 和 apply() 方法是预定义的 JavaScript 方法。

它们都可以用于将另一个对象作为参数调用对象方法。

您可以在本教程后面阅读有关 call() 和 apply() 的更多内容。

在下面的例子中，当使用 person2 作为参数调用 person1.fullName 时，this 将引用 person2，即使它是 person1 的方法：

### 实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript <b>this</b> 关键词</h1>

<p>在本例中，<strong>this</strong> 引用 person2，即使它是 person1 的方法：</p>

<p id="demo"></p>

<script>
var person1 = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}
var person2 = {
  firstName:"Bill",
  lastName: "Gates",
}
var x = person1.fullName.call(person2); 
document.getElementById("demo").innerHTML = x; 
</script>

</body>
</html>
```





# JavaScript Let



## 全局作用域

*全局*（在函数之外）声明的变量拥有*全局作用域*。

实例

```javascript
var carName = "porsche";

// 此处的代码可以使用 carName

function myFunction() {
  // 此处的代码也可以使用 carName
}
```



## 函数作用域

*局部*（函数内）声明的变量拥有*函数作用域*。

实例

```javascript
// 此处的代码不可以使用 carName

function myFunction() {
  var carName = "porsche";
  // code here CAN use carName
}

// 此处的代码不可以使用 carName
```



## JavaScript 块作用域

通过 var 关键词声明的变量没有块*作用域*。

在块 *{}* 内声明的变量可以从块之外进行访问。

实例

```javascript
{ 
  var x = 10; 
}
// 此处可以使用 x
```

在 ES2015 之前，JavaScript 是没有块作用域的。

可以使用 let 关键词声明拥有块作用域的变量。

在块 *{}* 内声明的变量无法从块外访问：

实例

```javascript
{ 
  let x = 10;
}
// 此处不可以使用 x
```



## 重新声明变量

使用 var 关键字重新声明变量会带来问题。

在块中重新声明变量也将重新声明块外的变量：

实例

```javascript
var x = 10;
// 此处 x 为 10
{ 
  var x = 6;
  // 此处 x 为 6
}
// 此处 x 为 6
```

使用 let 关键字重新声明变量可以解决这个问题。

在块中重新声明变量不会重新声明块外的变量：

实例

```javascript
var x = 10;
// 此处 x 为 10
{ 
  let x = 6;
  // 此处 x 为 6
}
// 此处 x 为 10
```



## 循环作用域

在循环中使用 var：

实例

```javascript
var i = 7;
for (var i = 0; i < 10; i++) {
  // 一些语句
}
// 此处，i 为 10
```

在循环中使用 let：

实例

```javascript
let i = 7;
for (let i = 0; i < 10; i++) {
  // 一些语句
}
// 此处 i 为 7
```





## HTML 中的全局变量

使用 JavaScript 的情况下，全局作用域是 JavaScript 环境。

在 HTML 中，全局作用域是 window 对象。

通过 var 关键词定义的全局变量属于 window 对象：

### 实例

```
var carName = "porsche";
// 此处的代码可使用 window.carName
```

通过 let 关键词定义的全局变量不属于 window 对象：

### 实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 全局变量</h1>

<p>在 HTML 中，由 <b>let</b> 定义的全局变量，不会成为 window 变量。</p>

<p id="demo"></p>

<script>
let carName = "Audi";

// 此处的代码能够使用 window.carName
document.getElementById("demo").innerHTML = "我不能显示 " + window.carName;
</script>

</body>
</html>
```



## 重新声明

允许在程序的任何位置使用 var 重新声明 JavaScript 变量：

实例

```javascript
var x = 10;

// 现在，x 为 10
 
var x = 6;

// 现在，x 为 6
```

在相同的作用域，或在相同的块中，通过 let 重新声明一个 var 变量是不允许的：

实例

```javascript
var x = 10;       // 允许
let x = 6;       // 不允许

{
  var x = 10;   // 允许
  let x = 6;   // 不允许
}
```

在相同的作用域，或在相同的块中，通过 let 重新声明一个 let 变量是不允许的：

实例

```javascript
let x = 10;       // 允许
let x = 6;       // 不允许

{
  let x = 10;   // 允许
  let x = 6;   // 不允许
}
```

在相同的作用域，或在相同的块中，通过 var 重新声明一个 let 变量是不允许的：

实例

```javascript
let x = 10;       // 允许
var x = 6;       // 不允许

{
  let x = 10;   // 允许
  var x = 6;   // 不允许
}
```

在不同的作用域或块中，通过 let 重新声明变量是允许的：

实例

```javascript
let x = 6;       // 允许

{
  let x = 7;   // 允许
}

{
  let x = 8;   // 允许
}
```

通过 let 定义的变量不会被提升到顶端。

在声明 let 变量之前就使用它会导致 ReferenceError。

变量从块的开头一直处于“暂时死区”，直到声明为止：

实例

```javascript
// 在此处，您不可以使用 carName
let carName;
```



# JavaScript Const

通过 const 定义的变量与 let 变量类似，但不能重新赋值：

## 块作用域

在*块作用域*内使用 const 声明的变量与 let 变量相似。

在本例中，x 在块中声明，不同于在块之外声明的 x：

实例

```javascript
var x = 10;
// 此处，x 为 10
{ 
  const x = 6;
  // 此处，x 为 6
}
// 此处，x 为 10
```



## 不是真正的常数

关键字 const 有一定的误导性。

它没有定义常量值。它定义了对值的常量引用。

因此，我们不能更改常量原始值，但我们可以**更改常量对象的属性**。



## 常量数组可以更改

您可以更改常量数组的元素：

实例

```javascript
// 您可以创建常量数组：
const cars = ["Audi", "BMW", "porsche"];

// 您可以更改元素：
cars[0] = "Honda";

// 您可以添加元素：
cars.push("Volvo");
```



## 重新声明

在程序中的任何位置都允许重新声明 JavaScript var 变量：

实例

```javascript
var x = 2;    //  允许
var x = 3;    //  允许
x = 4;        //  允许
```

在同一作用域或块中，不允许将已有的 var 或 let 变量重新声明或重新赋值给 const：

实例

```javascript
var x = 2;         // 允许
const x = 2;       // 不允许
{
  let x = 2;     // 允许
  const x = 2;   // 不允许
}
```

在同一作用域或块中，为已有的 const 变量重新声明声明或赋值是不允许的：

实例

```javascript
const x = 2;       // 允许
const x = 3;       // 不允许
x = 3;             // 不允许
var x = 3;         // 不允许
let x = 3;         // 不允许

{
  const x = 2;   // 允许
  const x = 3;   // 不允许
  x = 3;         // 不允许
  var x = 3;     // 不允许
  let x = 3;     // 不允许
}
```

在另外的作用域或块中重新声明 const 是允许的：

实例

```javascript
const x = 2;       // 允许

{
  const x = 3;   // 允许
}

{
  const x = 4;   // 允许
}
```



## 提升

Hoisting 是 JavaScript 将所有声明提升到当前作用域顶部的默认行为（提升到当前脚本或当前函数的顶部）。

用 let 或 const 声明的变量和常量不会被提升！



```js
var x = 5; // 初始化 x
 
elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x + " " + y;           // 显示 x 和 y
 
var y = 7; // 初始化 y 
```

因为只有声明（var y）而不是初始化（=7）被提升到顶部。

由于 hoisting，y 在其被使用前已经被声明，但是由于未对初始化进行提升，y 的值仍是未定义





# JavaScript Use Strict

**"use strict"; 定义 JavaScript 代码应该以“严格模式”执行。**

以下版本的浏览器支持严格模式：

- 版本 10 以后的 IE
- 版本 4 以后的 Firefox
- 版本 13 以后的 Chrome
- 版本 5.1 以后的 Safari
- 版本 12 以后的 Opera



## 声明严格模式

通过在脚本或函数的开头添加 "use strict"; 来声明严格模式。

在脚本开头进行声明，拥有全局作用域（脚本中的所有代码均以严格模式来执行）：

```js
"use strict";
myFunction();

function myFunction() {
     y = 3.14;   // 这会引发错误，因为 y 尚未声明
}
```

```js
x = 3.14;       // 这不会引发错误
myFunction();

function  myFunction() {
	"use strict";
	 y = 3.14;   // 这会引发错误
}
```

* 在不声明对象的情况下使用对象也是不允许的
* 删除变量（或对象）是不允许的

* 删除函数是不允许的
* 重复参数名是不允许的
* 八进制数值文本是不允许的
* 转义字符是不允许的
* 写入只读属性是不允许的
* 写入只能获取的属性是不允许的
* 删除不可删除的属性是不允许的
* 字符串 "eval" 不可用作变量
* 字符串 "arguments" 不可用作变量
* with 语句是不允许的
* 处于安全考虑，不允许 eval() 在其被调用的作用域中创建变量
* 在类似 f() 的函数调用中，this 的值是全局对象。在严格模式中，现在它成为了 undefined



## 对未来的保障

严格模式中不允许使用为未来预留的关键词。它们是：

- implements
- interface
- let
- package
- private
- protected
- public
- static
- yield



## 警告

"use strict" 指令只能在脚本或函数的*开头*被识别。