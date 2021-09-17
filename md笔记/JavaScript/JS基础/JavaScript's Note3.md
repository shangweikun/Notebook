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



## 方法中的 this

在对象方法中，this 指的是此方法的“拥有者”。

在本页最上面的例子中，this 指的是 person 对象。

person 对象是 fullName 方法的拥有者。

```javascript
fullName : function() {
  return this.firstName + " " + this.lastName;
}
```



## 单独的 this

在单独使用时，拥有者是全局对象，因此 this 指的是全局对象。

在浏览器窗口中，全局对象是 [object Window]：

### 实例

```javascript
var x = this;
```



在严格模式中，如果单独使用，那么 this 指的是全局对象 [object Window]：

### 实例

```javascript
"use strict";
var x = this;
```



## 事件处理程序中的 this

在 HTML 事件处理程序中，this 指的是接收此事件的 HTML 元素：

### 实例

```html
<button onclick="this.style.display='none'">
  点击来删除我！
</button>
```



## 对象方法绑定

在此例中，this 是 person 对象（person 对象是该函数的“拥有者”）：

### 实例

```javascript
var person = {
  firstName  : "Bill",
  lastName   : "Gates",
  id         : 678,
  myFunction : function() {
    return this;
  }
};
```



## 显式函数绑定

call() 和 apply() 方法是预定义的 JavaScript 方法。

它们都可以用于将另一个对象作为参数调用对象方法。

您可以在本教程后面阅读有关 call() 和 apply() 的更多内容。

在下面的例子中，当使用 person2 作为参数调用 person1.fullName 时，this 将引用 person2，即使它是 person1 的方法：

### 实例

```javascript
var person1 = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}
var person2 = {
  firstName:"Bill",
  lastName: "Gates",
}
person1.fullName.call(person2);  // 会返回 "Bill Gates"
```





#ECMAScript 2015

ES2015 引入了两个重要的 JavaScript 新关键词：let 和 const。

这两个关键字在 JavaScript 中提供了块作用域（*Block Scope*）变量（和常量）。

在 ES2015 之前，JavaScript 只有两种类型的作用域：*全局作用域*和*函数作用域*。



## 全局作用域

*全局*（在函数之外）声明的变量拥有*全局作用域*。

### 实例

```javascript
var carName = "porsche";

// 此处的代码可以使用 carName

function myFunction() {
  // 此处的代码也可以使用 carName
}
```





## JavaScript 块作用域

通过 var 关键词声明的变量没有块*作用域*。

在块 *{}* 内声明的变量可以从块之外进行访问。

### 实例

```javascript
{ 
  var x = 10; 
}
// 此处可以使用 x
```

在 ES2015 之前，JavaScript 是没有块作用域的。

可以使用 let 关键词声明拥有块作用域的变量。

在块 *{}* 内声明的变量无法从块外访问：

### 实例

```javascript
{ 
  let x = 10;
}
// 此处不可以使用 x
```

-----

##  重新声明变量

使用 var 关键字重新声明变量会带来问题。

在块中重新声明变量也将重新声明块外的变量：

### 实例

```javascript
var x = 10;
// 此处 x 为 10
{ 
  var x = 6;
  // 此处 x 为 6
}
// 此处 x 为 6
```



## 循环作用域

在循环中使用 var：

### 实例

```javascript
var i = 7;
for (var i = 0; i < 10; i++) {
  // 一些语句
}
// 此处，i 为 10
```

在循环中使用 let：

### 实例

```javascript
let i = 7;
for (let i = 0; i < 10; i++) {
  // 一些语句
}
// 此处 i 为 7
```



在第一个例子中，在循环中使用的变量使用 var 重新声明了循环之外的变量。

在第二个例子中，在循环中使用的变量使用 let 并没有重新声明循环外的变量。

如果在循环中用 let 声明了变量 i，那么只有在循环内，变量 i 才是可见的。



## 函数作用域

在函数内声明变量时，使用 var 和 let 很相似。

它们都有*函数作用域*：

```javascript
function myFunction() {
  var carName = "porsche";   // 函数作用域
}
function myFunction() {
  let carName = "porsche";   // 函数作用域
}
```



## 全局作用域

如果在块外声明声明，那么 var 和 let 也很相似。

它们都拥有*全局作用域*：

```javascript
var x = 10;       // 全局作用域
let y = 6;       // 全局作用域
```



## HTML 中的全局变量

使用 JavaScript 的情况下，全局作用域是 JavaScript 环境。

在 HTML 中，全局作用域是 window 对象。

通过 var 关键词定义的全局变量属于 window 对象：

### 实例

```javascript
var carName = "porsche";
// 此处的代码可使用 window.carName
```



通过 let 关键词定义的全局变量不属于 window 对象：

### 实例

```javascript
let carName = "porsche";
// 此处的代码不可使用 window.carName
```



## 重新声明

允许在程序的任何位置使用 var 重新声明 JavaScript 变量：

### 实例

```javascript
var x = 10;

// 现在，x 为 10
 
var x = 6;

// 现在，x 为 6
```



在相同的作用域，或在相同的块中，通过 let 重新声明一个 var 变量是不允许的：

### 实例

```javascript
var x = 10;       // 允许
let x = 6;       // 不允许

{
  var x = 10;   // 允许
  let x = 6;   // 不允许
}
```



在相同的作用域，或在相同的块中，通过 let 重新声明一个 let 变量是不允许的：

### 实例

```javascript
let x = 10;       // 允许
let x = 6;       // 不允许

{
  let x = 10;   // 允许
  let x = 6;   // 不允许
}
```



在相同的作用域，或在相同的块中，通过 var 重新声明一个 let 变量是不允许的：

### 实例

```javascript
let x = 10;       // 允许
var x = 6;       // 不允许

{
  let x = 10;   // 允许
  var x = 6;   // 不允许
}
```



在不同的作用域或块中，通过 let 重新声明变量是允许的：

### 实例

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

### 实例

```javascript
// 在此处，您不可以使用 carName
let carName;
```



# JavaScript Const

通过 `const` 定义的变量与 `let` 变量类似，但不能重新赋值：



## 块作用域

在*块作用域*内使用 const 声明的变量与 let 变量相似。

在本例中，x 在块中声明，不同于在块之外声明的 x：

### 实例

```javascript
var x = 10;
// 此处，x 为 10
{ 
  const x = 6;
  // 此处，x 为 6
}
// 此处，x 为 10
```



## 在声明时赋值

JavaScript const 变量必须在声明时赋值：

### 不正确

```javascript
const PI;
PI = 3.14159265359;
```

### 正确

```javascript
const PI = 3.14159265359;
```



## 常量对象可以更改

您可以更改常量对象的属性：但是您无法重新为常量对象赋值：

### 实例

```javascript
// 您可以创建 const 对象：
const car = {type:"porsche", model:"911", color:"Black"};

// 您可以更改属性：
car.color = "White";

// 您可以添加属性：
car.owner = "Bill";
```

```javascript
const car = {type:"porsche", model:"911", color:"Black"};
car = {type:"Volvo", model:"XC60", color:"White"};    // ERROR
```



## 常量数组可以更改

您可以更改常量数组的元素：但是您无法重新为常量数组赋值：

### 实例

```javascript
// 您可以创建常量数组：
const cars = ["Audi", "BMW", "porsche"];

// 您可以更改元素：
cars[0] = "Honda";

// 您可以添加元素：
cars.push("Volvo");
```

```javascript
const cars = ["Audi", "BMW", "porsche"];
cars = ["Honda", "Toyota", "Volvo"];    // ERROR
```



## 重新声明

在程序中的任何位置都允许重新声明 JavaScript var 变量：

### 实例

```javascript
var x = 2;    //  允许
var x = 3;    //  允许
x = 4;        //  允许
```

在同一作用域或块中，不允许将已有的 var 或 let 变量重新声明或重新赋值给 const：

### 实例

```javascript
var x = 2;         // 允许
const x = 2;       // 不允许
{
  let x = 2;     // 允许
  const x = 2;   // 不允许
}
```

在同一作用域或块中，为已有的 const 变量重新声明声明或赋值是不允许的：

### 实例

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

### 实例

```javascript
const x = 2;       // 允许

{
  const x = 3;   // 允许
}

{
  const x = 4;   // 允许
}
```



## 期望松散的比较

在常规比较中，数据类型不重要。这条 if 语句返回 true：

```javascript
var x = 10;
var y = "10";
if (x == y) 
```



在严格比较中，数据类型确实重要。这条 if 语句返回 false：

```javascript
var x = 10;
var y = "10";
if (x === y) 
```



有一个常见的错误是忘记在 switch 语句中使用严格比较：

这条 switch 语句会显示提示框：

```javascript
var x = 10;
switch(x) {
    case 10: alert("Hello");
}
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



## 对 JavaScript 字符串换行

如果必须在字符串中换行，则必须使用反斜杠：

### 例子 3

```javascript
var x = "Hello \
World!";
```



## 对 return 语句进行换行

在一行的结尾自动关闭语句是默认的 JavaScript 行为。

正因如此，下面两个例子返回相同的结果：

### 例子 1

```
function myFunction(a) {
    var power = 10  
    return a * power
}
```



### 例子 2

```
function myFunction(a) {
    var power = 10;
    return a * power;
}
```



JavaScript 也允许您将一条语句换行为两行。

正因如此，例子 3 也将返回相同的结果：

### 例子 3

```
function myFunction(a) {
    var
    power = 10;  
    return a * power;
}
```



但是，如果把 return 语句换行为两行会发生什么呢：

### 例子 4

```
function myFunction(a) {
    var
    power = 10;  
    return
    a * power;
}
```



此函数将返回 undefined！

为什么呢？因为 JavaScript 认为你的意思是：

### 例子 5

```javascript
function myFunction(a) {
     var
    power = 10;  
    return;
    a * power;
}
```



## 解释

如果某条语句是不完整的：

```javascript
var
```

JavaScript 将通过读取下一行来完成这条语句：

```javascript
power = 10;
```

但是由于这条语句是完整的：

```javascript
return
```

JavaScript 会自动关闭该语句：

```javascript
return;
```

发生这种情况是因为，在 JavaScript 中，用分号来关闭（结束）语句是可选的。

JavaScript 会在行末关闭 return 语句，因为它本身就是一条完整的语句。

所以，绝不要对 return 语句进行换行。



## 通过命名索引来访问数组

很多编程语言支持带有命名索引的数组。

带有命名索引的数组被称为关联数组（或散列）。

JavaScript *不支持*带有命名索引的数组。

在 JavaScript 中，*数组*使用*数字索引*：

### 实例

```javascript
var person = [];
person[0] = "Bill";
person[1] = "Gates";
person[2] = 46;
var x = person.length;          // person.length 将返回 3
var y = person[0];              // person[0] 将返回 "John"
```



## 用逗号来结束定义

对象和数组定义中的尾随逗号在 ECMAScript 5 中是合法的。

### 对象实例：

```javascript
person = {firstName:"Bill", lastName:"Gates", age:62,}
```

### 数组实例：

```javascript
points = [35, 450, 2, 7, 30, 16,];
```

### 警告！！

Internet Explorer 8 会崩溃。

JSON 不允许尾随逗号。

### JSON:

```javascript
person = {firstName:"Bill", lastName:"Gates", age:62}
```

### JSON:

```javascript
points = [35, 450, 2, 7, 30, 16];
```

## Undefined 不是 Null

JavaScript 对象、变量、属性和方法可以是未定义的。

此外，空的 JavaScript 对象的值可以为 null。

这可能会使测试对象是否为空变得有点困难。

您可以通过测试类型是否为 undefined，来测试对象是否存在：

### 实例

```javascript
if (typeof myObj === "undefined")
```



### 正确的：

```javascript
if (typeof myObj !== "undefined" && myObj !== null)
```



## 期望块级范围

JavaScript *不会*为每个代码块创建新的作用域。

很多编程语言都是如此，但是 JavaScript *并非如此*。

认为这段代码会返回 undefined，是新的 JavaScript 开发者的常见错误：

### 实例

```javascript
for (var i = 0; i < 10; i++) {
  // 代码块
}
return i;
```







# JavaScript 性能



**如何加速您的 JavaScript 代码。**



## 减少循环中的活动

编程经常会用到循环。

循环每迭代一次，循环中的每条语句，包括 for 语句，都会被执行。

能够放在循环之外的语句或赋值会使循环运行得更快。

差的代码：

```javascript
var i;
for (i = 0; i < arr.length; i++) {
```

更好的代码：

```javascript
var i;
var l = arr.length;
for (i = 0; i < l; i++) {
```

循环每次迭代时，坏代码就会访问数组的 length 属性。

好代码在循环之外访问 length 属性，使循环更快。



## 减少 DOM 访问

与其他 JavaScript 相比，访问 HTML DOM 非常缓慢。

假如您期望访问某个 DOM 元素若干次，那么只访问一次，并把它作为本地变量来使用：

### 实例

```javascript
var obj;
obj = document.getElementById("demo");
obj.innerHTML = "Hello"; 
```



## 缩减 DOM 规模

请尽量保持 HTML DOM 中较少的元素数量。

这么做总是会提高页面加载，并加快渲染（页面显示），尤其是在较小的设备上。

每一次试图搜索 DOM（比如 getElementsByTagName）都将受益于一个较小的 DOM。



## 避免不必要的变量

请不要创建不打算存储值的新变量。

通常您可以替换代码：

```javascript
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

一个选项是在 script 标签中使用 `defer="true"`。`defer `属性规定了脚本应该在页面完成解析后执行，但它只适用于外部脚本。

如果可能，您可以在页面完成加载后，通过代码向页面添加脚本：

### 实例

```javascript
<script>
window.onload = downScripts;

function downScripts() {
    var element = document.createElement("script");
    element.src = "myScript.js";
    document.body.appendChild(element);
}
</script>
```



## 避免使用 with

请避免使用 with 关键词。它对速度有负面影响。它也将混淆 JavaScript 作用域。

严格模式中*不允许* with 关键词。