# ECMAScript 5 - JavaScript 5



## ECMAScript 5 是什么？

ECMAScript 5 也称为 ES5 和 ECMAScript 2009。

本章介绍 ES5 的一些最重要的特性。

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

## ECMAScript 5 语法更改

- 对字符串的属性访问 [ ]
- 数组和对象字面量中的尾随逗号
- 多行字符串字面量
- 作为属性名称的保留字



## "use strict" 指令

“use strict” 定义 JavaScript 代码应该以“严格模式”执行。



## String.trim()

String.trim() 删除字符串两端的空白字符。

### 实例

```javascript
var str = "       Hello World!        ";
alert(str.trim());
```



## Array.isArray()

isArray() 方法检查对象是否为数组。

### 实例

```javascript
function myFunction() {
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  var x = document.getElementById("demo");
  x.innerHTML = Array.isArray(fruits);
}
```



## Array.forEach()

forEach() 方法为每个数组元素调用一次函数。

### 实例

```javascript
var txt = "";
var numbers = [45, 4, 9, 16, 25];
numbers.forEach(myFunction);

function myFunction(value) {
  txt = txt + value + "<br>"; 
}
```



## Array.map()

这个例子给每个数组值乘以 2：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var numbers2 = numbers1.map(myFunction);

function myFunction(value) {
  return value * 2;
}
```



## Array.filter()

此例用值大于 18 的元素创建一个新数组：

### 实例

```javascript
var numbers = [45, 4, 9, 16, 25];
var over18 = numbers.filter(myFunction);

function myFunction(value) {
  return value > 18;
}
```



## Array.reduce()

这个例子确定数组中所有数的总和：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduce(myFunction);

function myFunction(total, value) {
  return total + value;
}
```



## Array.reduceRight()

这个例子同样是确定数组中所有数的总和：

### 实例

```javascript
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduceRight(myFunction);

function myFunction(total, value) {
  return total + value;
}
```



## Array.every()

这个例子检查是否所有值都超过 18：

### 实例

```javascript
var numbers = [45, 4, 9, 16, 25];
var allOver18 = numbers.every(myFunction);

function myFunction(value) {
  return value > 18;
}
```



## Array.some()

这个例子检查某些值是否超过 18：

### 实例

```javascript
var numbers = [45, 4, 9, 16, 25];
var allOver18 = numbers.some(myFunction);

function myFunction(value) {
  return value > 18;
}
```



## Array.indexOf()

检索数组中的某个元素值并返回其位置：

### 实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
```



## Array.lastIndexOf()

Array.lastIndexOf() 与 Array.indexOf() 类似，但是从数组结尾处开始检索。

### 实例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.lastIndexOf("Apple");
```



## JSON.parse()

JSON 的一个常见用途是从 Web 服务器接收数据。

想象一下，您从Web服务器收到这条文本字符串：

```
'{"name":"Bill", "age":62, "city":"Seatle"}'
```

JavaScript 函数 JSON.parse() 用于将文本转换为 JavaScript 对象：

```javascript
var obj = JSON.parse('{"name":"Bill", "age":62, "city":"Seatle"}');
```



## JSON.stringify()

JSON 的一个常见用途是将数据发送到Web服务器。

将数据发送到 Web 服务器时，数据必须是字符串。

想象一下，我们在 JavaScript 中有这个对象：

```javascript
var obj = {"name":"Bill", "age":62, "city":"Seatle"};
```

请使用 JavaScript 函数 JSON.stringify() 将其转换为字符串。

```javascript
var myJSON = JSON.stringify(obj);
```

结果将是遵循 JSON 表示法的字符串。

myJSON 现在是一个字符串，准备好发送到服务器：

### 实例

```javascript
var obj = {"name":"Bill", "age":62, "city":"Seatle"};
var myJSON = JSON.stringify(obj);
document.getElementById("demo").innerHTML = myJSON;
```





## Date.now()

Date.now() 返回自零日期（1970 年 1 月 1 日 00:00:00:00）以来的毫秒数。

### 实例

```javascript
var timInMSs = Date.now();
```



## 属性 Getter 和 Setter

ES5 允许您使用类似于获取或设置属性的语法来定义对象方法。

这个例子为名为 fullName 的属性创建一个 *getter*：

### 实例

```javascript
// 创建对象：
var person = {
  firstName: "Bill",
  lastName : "Gates",
  get fullName() {
    return this.firstName + " " + this.lastName;
  }
};

// 使用 getter 显示来自对象的数据：
document.getElementById("demo").innerHTML = person.fullName;
```



### 实例

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "NO",
  get lang() {
    return this.language;
  },
  set lang(value) {
    this.language = value;
  }
};

// 使用 setter 设置对象属性：
person.lang = "en";

// 使用 getter 显示来自对象的数据：
document.getElementById("demo").innerHTML = person.lang;
```



这个例子使用 setter 来确保语言的大写更新：

### 实例

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "NO",
  set lang(value) {
    this.language = value.toUpperCase();
  }
};

// 使用 setter 设置对象属性：
person.lang = "en";

// 显示来自对象的数据：
document.getElementById("demo").innerHTML = person.language;
```



## 新的对象属性和方法

Object.defineProperty() 是 ES5 中的新对象方法。

它允许您定义对象属性和/或更改属性的值和/或元数据。

### 实例

```javascript
// 创建对象：
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "NO", 
};

// 更改属性：
Object.defineProperty(person, "language", {
  value: "EN",
  writable : true,
  enumerable : true,
  configurable : true
});

// 枚举属性
var txt = "";
for (var x in person) {
  txt += person[x] + "<br>";
}
document.getElementById("demo").innerHTML = txt;
```



下一个例子是相同的代码，但它隐藏了枚举中的语言属性：

### 实例

```javascript
// 创建对象：
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "NO", 
};

// 更改属性：
Object.defineProperty(person, "language", {
  value: "EN",
  writable : true,
  enumerable : false,
  configurable : true
});

// 枚举属性
var txt = "";
for (var x in person) {
  txt += person[x] + "<br>";
}
document.getElementById("demo").innerHTML = txt;
```



此例创建一个 setter 和 getter 来确保语言的大写更新：

### 实例

```javascript
// 创建对象：
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "NO"
};

// 更改属性：
Object.defineProperty(person, "language", {
  get : function() { return language },
  set : function(value) { language = value.toUpperCase()}
});

// 更改语言
person.language = "en";

// 显示语言
document.getElementById("demo").innerHTML = person.language;
```





ECMAScript 5 为 JavaScript 添加了许多新的对象方法：

```javascript
ES5 新的对象方法

// 添加或更改对象属性
Object.defineProperty(object, property, descriptor)

// 添加或更改多个对象属性
Object.defineProperties(object, descriptors)

// 访问属性
Object.getOwnPropertyDescriptor(object, property)

// 将所有属性作为数组返回
Object.getOwnPropertyNames(object)

// 将可枚举属性作为数组返回
Object.keys(object)

// 访问原型
Object.getPrototypeOf(object)

// 防止向对象添加属性
Object.preventExtensions(object)

// 如果可以将属性添加到对象，则返回 true
Object.isExtensible(object)

// 防止更改对象属性（而不是值）
Object.seal(object)

// 如果对象被密封，则返回 true
Object.isSealed(object)

// 防止对对象进行任何更改
Object.freeze(object)

// 如果对象被冻结，则返回 true
Object.isFrozen(object)
```



## 对字符串的属性访问

charAt() 方法返回字符串中指定索引（位置）的字符：

### 实例

```javascript
var str = "HELLO WORLD";
str.charAt(0);            // 返回 H
```



ECMAScript 5 允许对字符串进行属性访问：

### 实例

```javascript
var str = "HELLO WORLD";
```



## 对字符串的属性访问

charAt() 方法返回字符串中指定索引（位置）的字符：

### 实例

```javascript
var str = "HELLO WORLD";
str.charAt(0);            // 返回 H
```



ECMAScript 5 允许对字符串进行属性访问：

### 实例

```javascript
var str = "HELLO WORLD";
str[0];                   // 返回 H
```





## 尾随逗号(Trailing Commas)

ECMAScript 5 允许在对象和数组定义中使用尾随逗号：

### Object 实例

```javascript
person = {
  firstName: "Bill",
  lastName: " Gates",
  age: 62,
}
```

### Array 实例

```javascript
points = [
  1,
  5,
  10,
  25,
  40,
  100,
];
```

### 警告！！！

Internet Explorer 8 将崩溃。

JSON 不允许使用尾随逗号。

### JSON 对象：

```javascript
// 允许：
var person = '{"firstName":"Bill", "lastName":"Gates", "age":62}'
JSON.parse(person)

// 不允许：
var person = '{"firstName":"Bill", "lastName":"Gates", "age":62,}'
JSON.parse(person)
```

### JSON 数组：

```javascript
// 允许：
points = [40, 100, 1, 5, 25, 10]

// 不允许：
points = [40, 100, 1, 5, 25, 10,]
```





## 多行字符串

如果使用反斜杠转义，ECMAScript 5 允许多行的字符串文字（字面量）：

### 实例

```javascript
"Hello \
Kitty!";
```



## 保留字作为属性名称

ECMAScript 5允许保留字作为属性名称：

### 对象实例

```javascript
var obj = {name: "Bill", new: "yes"}
```



参看：https://www.w3school.com.cn/js/js_es5.asp





# ECMAScript 6 - ECMAScript 2015



## ECMAScript 6 是什么？

ECMAScript 6 也称为 ES6 和 ECMAScript 2015。

一些人把它称作 JavaScript 6。

本章介绍 ES6 中的一些新特性。

- JavaScript let
- JavaScript const
- 幂 (**)
- 默认参数值
- Array.find()
- Array.findIndex()



## JavaScript let

let 语句允许您使用块作用域声明变量。

### 实例

```javascript
var x = 10;
// Here x is 10
{ 
  let x = 2;
  // Here x is 2
}
// Here x is 10
```



## JavaScript const

const 语句允许您声明常量（具有常量值的 JavaScript 变量）。

常量类似于 let 变量，但不能更改值。

### 实例

```javascript
var x = 10;
// Here x is 10
{ 
  const x = 2;
  // Here x is 2
}
// Here x is 10
```



## 指数运算符

取幂运算符（**）将第一个操作数提升到第二个操作数的幂。

### 实例

```javascript
var x = 5;
var z = x ** 2;          // 结果是 25
```



x ** y 的结果与 Math.pow(x,y) 相同：

### 实例

```javascript
var x = 5;
var z = Math.pow(x,2);   // 结果是 25
```



## 默认参数值

ES6 允许函数参数具有默认值。

### 实例

```javascript
function myFunction(x, y = 10) {
  // y is 10 if not passed or undefined
  return x + y;
}
myFunction(5); // 将返回 15
```



## Array.find()

find() 方法返回通过测试函数的第一个数组元素的值。

此例查找（返回）第一个大于 18 的元素（的值）：

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

此例确定大于 18 的第一个元素的索引：

### 实例

```javascript
var numbers = [4, 9, 16, 25, 29];
var first = numbers.findIndex(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```



请注意此函数接受 3 个参数：

- 项目值
- 项目索引
- 数组本身





## 新的数字属性

ES6 将以下属性添加到 Number 对象：

- EPSILON
- MIN_SAFE_INTEGER
- MAX_SAFE_INTEGER



## 新的数字方法

ES6 为 Number 对象添加了 2 个新方法：

- Number.isInteger()
- Number.isSafeInteger()

###Number.isInteger() 方法

如果参数是整数，则 Number.isInteger() 方法返回 true。

```javascript
Number.isInteger(10);        // 返回 true
Number.isInteger(10.5);      // 返回 false
```



###Number.isSafeInteger() 方法

安全整数是可以精确表示为双精度数的整数。

如果参数是安全整数，则 Number.isSafeInteger() 方法返回 true。

```javascript
Number.isSafeInteger(10);    // 返回 true
Number.isSafeInteger(12345678901234567890);  // 返回 false
```



安全整数指的是从 -(253 - 1) 到 +(253 - 1) 的所有整数。

这是安全的：9007199254740991。这是不安全的：9007199254740992。



## 新的全局方法

ES6 还增加了 2 个新的全局数字方法：

- isFinite()
- isNaN()

###isFinite() 方法

如果参数为 Infinity 或 NaN，则全局 isFinite() 方法返回 false。

否则返回 true：

```javascript
isFinite(10/0);       // 返回 false
isFinite(10/1);       // 返回 true
```



###isNaN() 方法

如果参数是 NaN，则全局 isNaN() 方法返回 true。否则返回 false：

```javascript
isNaN("Hello");       // 返回 true
```



## 箭头函数（Arrow Function）

箭头函数允许使用简短的语法来编写函数表达式。

您不需要 function 关键字、return 关键字以及*花括号*。

### 实例

```javascript
// ES5
var x = function(x, y) {
   return x * y;
}

// ES6
const x = (x, y) => x * y;
```



箭头功能没有自己的 this。它们不适合定义*对象方法*。

箭头功能未被提升。它们必须在使用*前*进行定义。

使用 const 比使用 var 更安全，因为函数表达式始终是常量值。

如果函数是单个语句，则只能省略 return 关键字和花括号。因此，保留它们可能是一个好习惯：

### 实例

```javascript
const x = (x, y) => { return x * y };
```