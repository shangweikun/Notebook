# JavaScript 数组

```javascript
var cars = ["Saab", "Volvo", "BMW"];
```

## 使用 JavaScript 关键词 new

下面的例子也会创建数组，并为其赋值：

实例

```javascript
var cars = new Array("Saab", "Volvo", "BMW");
```



输出所有的数组内容

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组</h1>

<p id="demo"></p>

<script>
var cars = ["Audi", "BMW", "porsche"];
document.getElementById("demo").innerHTML = cars;
</script>

</body>
</html>
```



## 数组是对象

数组是一种特殊类型的对象。在 JavaScript 中对数组使用 typeof 运算符会返回 "object"。

但是，JavaScript 数组最好以数组来描述。



## 数组元素可以是对象

JavaScript 变量可以是对象。数组是特殊类型的对象。

正因如此，您可以在相同数组中存放不同类型的变量。

您可以在数组保存对象。您可以在数组中保存函数。你甚至可以在数组中保存数组：

```javascript
myArray[0] = Date.now;
myArray[1] = myFunction;
myArray[2] = myCars;
```



您也可以使用 Array.foreach() 函数：

实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组</h1>

<p>Array.forEach() 为每个数组元素调用函数。</p>

<p>Internet Explorer 8 以及更早的版本不支持 Array.forEach()。</p>

<p id="demo"></p>

<script>
var fruits, text;
fruits = ["Banana", "Orange", "Apple", "Mango"];

text = "<ul>";
fruits.forEach(myFunction);
text += "</ul>";
document.getElementById("demo").innerHTML = text;

function myFunction(value) {
  text += "<li>" + value + "</li>";
} 
</script>

</body>
</html>
```



## 添加数组元素

向数组添加新元素的最佳方法是使用 push() 方法：

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组</h1>

<p>push 方法向数组追加新元素。</p>

<button onclick="myFunction()">试一试</button>

<p id="demo"></p>

<script>
var fruits = ["Banana", "Orange", "Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits;

function myFunction() {
  fruits.push("Lemon");
  document.getElementById("demo").innerHTML = fruits;
}
</script>

</body>
</html>

```



### 警告！

添加最高索引的元素可在数组中创建未定义的“洞”：

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组</h1>

<p>添加具有高索引的元素可以在数组中创建未定义的“孔”。</p>

<p id="demo"></p>

<script>
var fruits, text, fLen, i;
fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits[6] = "Lemon";

fLen = fruits.length;
text = "";
for (i = 0; i < fLen; i++) {
  text += fruits[i] + "<br>";
}
document.getElementById("demo").innerHTML = text;
</script>

</body>
</html>
```



## 关联数组

很多编程元素支持命名索引的数组。

具有命名索引的数组被称为关联数组（或散列）。

JavaScript *不支持*命名索引的数组。

在 JavaScript 中，数组只能使用*数字索引*。



## 数组和对象的区别

在 JavaScript 中，*数组*使用*数字索引*。

在 JavaScript 中，*对象*使用*命名索引*。

数组是特殊类型的对象，具有数字索引。



## 避免 new Array()

没有必要使用 JavaScript 的内建数组构造器 new Array()。

*请使用 [] 取而代之！*

下面两条不同的语句创建了名为 points 的新的空数组：

```javascript
var points = new Array();         // 差
var points = [];                  // 优
```

下面两条不同的语句创建包含六个数字的新数组：

```javascript
var points = new Array(40, 100, 1, 5, 25, 10); // 差
var points = [40, 100, 1, 5, 25, 10];          // 优
```



## 如何识别数组

ECMAScript 5 定义了新方法 Array.isArray()：

```javascript
Array.isArray(fruits);     // 返回 true
```

创建您自己的 isArray() 函数以解决此问题：

```javascript
function isArray(x) {
    return x.constructor.toString().indexOf("Array") > -1;
}
```

假如对象由给定的构造器创建，则 *instanceof* 运算符返回 true：

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
 
fruits instanceof Array     // 返回 true
```





# JavaScript 数组方法

## 把数组转换为字符串

JavaScript 方法 toString() 把数组转换为数组值（逗号分隔）的字符串。

实例

```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.toString(); 
```



###join() 方法

也可将所有数组元素结合为一个字符串。

它的行为类似 toString()，但是您还可以规定分隔符：

实例

```javascript
var fruits = ["Banana", "Orange","Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.join(" * "); 
```





## Popping 和 Pushing

在处理数组时，删除元素和添加新元素是很简单的。

Popping 和 Pushing 指的是：

从数组*弹出*项目，或向数组*推入*项目。

pop() 方法从数组中删除最后一个元素：

push() 方法（在数组结尾处）向数组添加一个新的元素：push() 方法返回新数组的长度：



## 位移元素

位移与弹出等同，但处理首个元素而不是最后一个。

shift() 方法会删除首个数组元素，并把所有其他元素“位移”到更低的索引。

shift() 方法返回被“位移出”的字符串：

unshift() 方法（在开头）向数组添加新元素，并“反向位移”旧元素：

unshift() 方法返回新数组的长度。



## 删除元素

既然 JavaScript 数组属于对象，其中的元素就可以使用 JavaScript delete 运算符来*删除*：

使用 delete 会在数组留下未定义的空洞。请使用 pop() 或 shift() 取而代之。



## 拼接数组

splice() 方法可用于向数组添加新项：

实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 0, "Lemon", "Kiwi");
```

第一个参数（2）定义了应添加新元素的位置（拼接）。

第二个参数（0）定义应删除多少元素。

其余参数（“Lemon”，“Kiwi”）定义要添加的新元素。

splice() 方法返回一个包含已删除项的数组：

实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 2, "Lemon", "Kiwi");
```



## 使用 splice() 来删除元素

通过聪明的参数设定，您能够使用 splice() 在数组中不留“空洞”的情况下移除元素：

实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(0, 1);        // 删除 fruits 中的第一个元素
```

第一个参数（0）定义新元素应该被*添加*（接入）的位置。

第二个参数（1）定义应该*删除多个*元素。

其余参数被省略。没有新元素将被添加。



## 合并（连接）数组

concat() 方法通过合并（连接）现有数组来创建一个新数组：

实例（合并两个数组）

```javascript
var myGirls = ["Cecilie", "Lone"];
var myBoys = ["Emil", "Tobias", "Linus"];
var myChildren = myGirls.concat(myBoys);   // 连接 myGirls 和 myBoys
```

concat() 方法不会更改现有数组。它总是返回一个新数组。

concat() 方法可以使用任意数量的数组参数：

### 实例（合并三个数组）

```javascript
var arr1 = ["Cecilie", "Lone"];
var arr2 = ["Emil", "Tobias", "Linus"];
var arr3 = ["Robin", "Morgan"];
var myChildren = arr1.concat(arr2, arr3);   // 将arr1、arr2 与 arr3 连接在一起
```

concat() 方法也可以将值作为参数：

### 实例（将数组与值合并）

```javascript
var arr1 = ["Cecilie", "Lone"];
var myChildren = arr1.concat(["Emil", "Tobias", "Linus"]); 
```



## 裁剪数组

slice() 方法用数组的某个片段切出新数组。

本例从数组元素 1 （"Orange"）开始切出一段数组：

slice() 方法创建新数组。它不会从源数组中删除任何元素。

### 实例

```javascript
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1); 
```

slice() 可接受两个参数，比如 (1, 3)。

该方法会从开始参数选取元素，直到结束参数（不包括）为止。

### 实例

```javascript
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1, 3); 
```



## 自动 toString()

如果需要原始值，则 JavaScript 会自动把数组转换为字符串。下面两个例子将产生相同的结果：

### 实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组方法</h1> 

<h2>toString()</h2>

<p>toString() 方法以逗号分隔的字符串返回数组：</p>

<p id="demo"></p>
<p id="demo1"></p>

<script>
var fruits = ["Banana", "Orange", "Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.toString();
document.getElementById("demo1").innerHTML = fruits;

</script>

</body>
</html>
```



**sort() 方法是最强大的数组方法之一。**

## 数组排序

sort() 方法以字母顺序对数组进行排序：

### 实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
```



## 反转数组

reverse() 方法反转数组中的元素。

您可以使用它以降序对数组进行排序：

### 实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
fruits.reverse();         // 反转元素顺序
```



## 数字排序

默认地，sort() 函数按照*字符串*顺序对值进行排序。

该函数很适合字符串（"Apple" 会排在 "Banana" 之前）。

不过，如果数字按照字符串来排序，则 "25" 大于 "100"，因为 "2" 大于 "1"。

正因如此，sort() 方法在对数值排序时会产生不正确的结果。

我们通过一个*比值函数*来修正此问题：

### 实例

```javascript
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a - b}); 
```



使用相同的技巧对数组进行降序排序：

### 实例

```javascript
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return b - a}); 
```



```html

<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组排序</h1>

<p>单击按钮可按字母顺序或数字顺序对数组进行排序。</p>

<button onclick="myFunction1()">按字母排序</button>
<button onclick="myFunction2()">按数字排序</button>

<p id="demo"></p>

<script>
var points = [40, 100, 1, 5, 25, 10];
document.getElementById("demo").innerHTML = points;  

function myFunction1() {
  points.sort();
  document.getElementById("demo").innerHTML = points;
}
function myFunction2() {
  points.sort(function(a, b){return a - b});
  document.getElementById("demo").innerHTML = points;
}
</script>

</body>
</html>

```



## 以随机顺序排序数组

### 实例

```javascript
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return 0.5 - Math.random()}); 
```



## 对数组使用 Math.max()/min()

您可以使用 Math.max.apply 来查找数组中的最高值：

### 实例

```javascript
function myArrayMax(arr) {
    return Math.max.apply(null, arr);
}
```

Math.max.apply([1, 2, 3]) 等于 Math.max(1, 2, 3)。

Math.min.apply([1, 2, 3]) 等于 Math.min(1, 2, 3)。



## 排序对象数组

JavaScript 数组经常会包含对象：

### 实例

```javascript
var cars = [
{type:"Volvo", year:2016},
{type:"Saab", year:2001},
{type:"BMW", year:2010}];
```

```javascript
cars.sort(function(a, b){return a.year - b.year});
```

比较字符串属性会稍复杂：

### 实例

```javascript
cars.sort(function(a, b){
	  var x = a.type.toLowerCase();
	  var y = b.type.toLowerCase();
	  if (x < y) {return -1;}
	  if (x > y) {return 1;}
	  return 0;
});
```



## Array.forEach()

forEach() 方法为每个数组元素调用一次函数（回调函数）。

### 实例

```javascript
var txt = "";
var numbers = [45, 4, 9, 16, 25];
numbers.forEach(myFunction);

function myFunction(value, index, array) {
  txt = txt + value + "<br>"; 
}
```

**注释：**该函数接受 3 个参数：

- 项目值
- 项目索引
- 数组本身



## Array.map()

map() 方法通过对每个数组元素执行函数来创建新数组。

map() 方法不会对没有值的数组元素执行函数。

map() 方法不会更改原始数组。

这个例子将每个数组值乘以2：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var numbers2 = numbers1.map(myFunction);

function myFunction(value, index, array) {
  return value * 2;
}
```

请注意，该函数有 3 个参数：

- 项目值
- 项目索引
- 数组本身

当回调函数仅使用 value 参数时，可以省略索引和数组参数



## Array.filter()

filter() 方法创建一个包含通过测试的数组元素的新数组。

这个例子用值大于 18 的元素创建一个新数组：

### 实例

```javascript
var numbers = [45, 4, 9, 16, 25];
var over18 = numbers.filter(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```

请注意此函数接受 3 个参数：

- 项目值
- 项目索引
- 数组本身

在上面的例子中，回调函数不使用 index 和 array 参数，因此可以省略它们





## Array.reduce()

reduce() 方法在每个数组元素上运行函数，以生成（减少它）单个值。

reduce() 方法在数组中从左到右工作。另请参见 reduceRight（）。

reduce() 方法不会减少原始数组。

这个例子确定数组中所有数字的总和：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduce(myFunction);

function myFunction(total, value, index, array) {
  return total + value;
}
```

请注意此函数接受 4 个参数：

- 总数（初始值/先前返回的值）
- 项目值
- 项目索引
- 数组本身

上例并未使用 index 和 array 参数。可以将它改写为：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduce(myFunction);

function myFunction(total, value) {
  return total + value;
}
```

reduce() 方法能够接受一个初始值：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduce(myFunction, 100);

function myFunction(total, value) {
  return total + value;
}
```

## Array.reduceRight()

## Array.every()

every() 方法检查所有数组值是否通过测试。

返回一个boolean值

这个例子检查所有数组值是否大于 18：

### 实例

```javascript
var numbers = [45, 4, 9, 16, 25];
var allOver18 = numbers.every(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```

请注意此函数接受 3 个参数：

- 项目值
- 项目索引
- 数组本身

如果回调函数仅使用第一个参数（值）时，可以省略其他参数：

### 实例

```
var numbers = [45, 4, 9, 16, 25];
var allOver18 = numbers.every(myFunction);

function myFunction(value) {
  return value > 18;
}
```



## Array.some()

some() 方法检查某些数组值是否通过了测试。

这个例子检查某些数组值是否大于 18：

### 实例

```javascript
var numbers = [45, 4, 9, 16, 25];
var someOver18 = numbers.some(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```



## Array.indexOf()

indexOf() 方法在数组中搜索元素值并返回其位置。

**注释：**第一个项目的位置是 0，第二个项目的位置是 1，以此类推。

### 实例

检索数组中的项目 "Apple"：

```javascript
var fruits = ["Apple", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
```

### 语法

```javascript
array.indexOf(item, start)
```

| *item*  | 必需。要检索的项目。                                         |
| ------- | ------------------------------------------------------------ |
| *start* | 可选。从哪里开始搜索。负值将从结尾开始的给定位置开始，并搜索到结尾。 |



## Array.lastIndexOf()

Array.lastIndexOf() 与 Array.indexOf() 类似，但是从数组结尾开始搜索。

### 实例

检索数组中的项目 "Apple"：

```javascript
var fruits = ["Apple", "Orange", "Apple", "Mango"];
var a = fruits.lastIndexOf("Apple");
```

### 语法

```javascript
array.lastIndexOf(item, start)
```

| *item*  | 必需。要检索的项目。                                         |
| ------- | ------------------------------------------------------------ |
| *start* | 可选。从哪里开始搜索。负值将从结尾开始的给定位置开始，并搜索到开头。 |



## Array.find()

find() 方法返回通过测试函数的第一个数组元素的值。

这个例子查找（返回）大于 18 的第一个元素的值：

### 实例

```javascript
var numbers = [4, 9, 16, 25, 29];
var first = numbers.find(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```

请注意此函数接受 3 个参数：

- 项目值
- 项目索引
- 数组本身



## Array.findIndex()

findIndex() 方法返回通过测试函数的第一个数组元素的索引。

这个例子查找大于 18 的第一个元素的索引：

### 实例

```javascript
var numbers = [4, 9, 16, 25, 29];
var first = numbers.findIndex(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```





# JavaScript 日期

```javascript
var d = new Date();
```

## 创建 Date 对象

Date 对象由新的 Date() 构造函数创建。

有 4 种方法创建新的日期对象：

- new Date()
- new Date(year, month, day, hours, minutes, seconds, milliseconds)
- new Date(milliseconds)
- new Date(date string)



您不能省略月份。如果只提供一个参数，则将其视为毫秒。

### 实例

```javascript
var d = new Date(2018);
```



一位和两位数年份将被解释为 19xx 年：

### 实例

```javascript
var d = new Date(99, 11, 24);
```



## new Date(dateString)

new Date(dateString) 从日期字符串创建一个新的日期对象：

### 实例

```javascript
var d = new Date("October 13, 2014 11:13:00");
```



## JavaScript 将日期存储为毫秒

JavaScript 将日期存储为自 1970 年 1 月 1 日 00:00:00 UTC（协调世界时）以来的毫秒数。

零时间是 1970 年 1 月 1 日 00:00:00 UTC。

现在的时间是：1970 年 1 月 1 日之后的 1554166879383 毫秒。



```javascript
var d = new Date(86400000);
```

一天（24 小时）是 86 400 000 毫秒。





toUTCString() 方法将日期转换为 UTC 字符串（一种日期显示标准）。

### 实例

```javascript
var d = new Date();
document.getElementById("demo").innerHTML = d.toUTCString();
```

```
Sun, 16 Aug 2020 11:36:29 GMT
```



toDateString() 方法将日期转换为更易读的格式：

### 实例

```javascript
var d = new Date();
document.getElementById("demo").innerHTML = d.toDateString();
```

```
Sun Aug 16 2020
```



##有四种 JavaScript 日期输入格式：

| 类型     | 实例                             |
| :------- | :------------------------------- |
| ISO 日期 | "2018-02-19" （国际标准）        |
| 短日期   | "02/19/2018" 或者 "2018/02/19"   |
| 长日期   | "Feb 19 2018" 或者 "19 Feb 2019" |
| 完整日期 | "Monday February 25 2015"        |

```javascript
var d = new Date("2018-02-19");
```

## 时区

在设置日期时，如果不规定时区，则 JavaScript 会使用浏览器的时区。

当获取日期时，如果不规定时区，则结果会被转换为浏览器时区。

## 警告

在某些浏览器中，不带前导零的月或其会产生错误：

```javascript
var d = new Date("2018-2-19");
```

“YYYY / MM / DD”的行为未定义。

有些浏览器会尝试猜测格式。有些会返回 NaN。

```javascript
var d = new Date("2018/02/19");
```

“DD-MM-YYYY”的行为也是未定义的。

有些浏览器会尝试猜测格式。有些会返回 NaN。

```javascript
var d = new Date("19-02-2018");
```



## JavaScript 长日期

长日期通常以 "MMM DD YYYY" 这样的语法来写：

月和天能够以任意顺序出现：

```javascript
var d = new Date("19 Feb 2018");
```

```javascript
var d = new Date("February 19 2018");
```

逗号会被忽略，且对大小写不敏感：

```javascript
var d = new Date("FEBRUARY, 25, 2015");
```



## 日期获取方法

获取方法用于获取日期的某个部分（来自日期对象的信息）。下面是最常用的方法（以字母顺序排序）：

| 方法              | 描述                                 |
| :---------------- | :----------------------------------- |
| getDate()         | 以数值返回天（1-31）                 |
| getDay()          | 以数值获取周名（0-6）                |
| getFullYear()     | 获取四位的年（yyyy）                 |
| getHours()        | 获取小时（0-23）                     |
| getMilliseconds() | 获取毫秒（0-999）                    |
| getMinutes()      | 获取分（0-59）                       |
| getMonth()        | 获取月（0-11）                       |
| getSeconds()      | 获取秒（0-59）                       |
| getTime()         | 获取时间（从 1970 年 1 月 1 日至今） |

### 实例

```javascript
var d = new Date();
document.getElementById("demo").innerHTML = d.getTime();
```



## UTC 日期方法

UTC 日期方法用于处理 UTC 日期（通用时区日期，Univeral Time Zone dates）：

| 方法                 | 描述                                    |
| :------------------- | :-------------------------------------- |
| getUTCDate()         | 等于 getDate()，但返回 UTC 日期         |
| getUTCDay()          | 等于 getDay()，但返回 UTC 日            |
| getUTCFullYear()     | 等于 getFullYear()，但返回 UTC 年       |
| getUTCHours()        | 等于 getHours()，但返回 UTC 小时        |
| getUTCMilliseconds() | 等于 getMilliseconds()，但返回 UTC 毫秒 |
| getUTCMinutes()      | 等于 getMinutes()，但返回 UTC 分        |
| getUTCMonth()        | 等于 getMonth()，但返回 UTC 月          |
| getUTCSeconds()      | 等于 getSeconds()，但返回 UTC 秒        |



## 日期设置方法

设置方法用于设置日期的某个部分。下面是最常用的方法（按照字母顺序排序）：

| 方法              | 描述                                         |
| :---------------- | :------------------------------------------- |
| setDate()         | 以数值（1-31）设置日                         |
| setFullYear()     | 设置年（可选月和日）                         |
| setHours()        | 设置小时（0-23）                             |
| setMilliseconds() | 设置毫秒（0-999）                            |
| setMinutes()      | 设置分（0-59）                               |
| setMonth()        | 设置月（0-11）                               |
| setSeconds()      | 设置秒（0-59）                               |
| setTime()         | 设置时间（从 1970 年 1 月 1 日至今的毫秒数） |