# JavaScript 函数定义



**JavaScript 函数是通过 function 关键词定义的。**

**您可以使用函数声明或函数表达式。**





## 函数声明

在本教程中稍早之前，您学到了通过如下语法*声明*函数：

```javascript
function functionName(parameters) {
   要执行的代码
}
```

被声明的函数不会直接执行。它们被“保存供稍后使用”，将在稍后执行，当它们被调用时。

### 实例

```javascript
function myFunction(a, b) {
     return a * b;
}
```

分号用于分隔可执行的 JavaScript 语句。

由于函数*声明*不是可执行的语句，以分号结尾并不常见。



## 函数表达式

JavaScript 函数也可以使用*表达式*来定义。

函数表达式可以在变量中存储：

### 实例

```javascript
var x = function (a, b) {return a * b};
```



在变量中保存函数表达式之后，此变量可用作函数：

### 实例

```javascript
var x = function (a, b) {return a * b};
var z = x(4, 3);
```



上面的函数实际上是一个*匿名函数*（没有名称的函数）。

存放在变量中的函数不需要函数名。他们总是使用变量名调用。

上面的函数使用分号结尾，因为它是可执行语句的一部分。



## Function() 构造器

正如您在之前的例子中看到的，JavaScript 函数是通过 function 关键词定义的。

函数也可以通过名为 Function() 的内建 JavaScript 函数构造器来定义。

### 实例

```javascript
var myFunction = new Function("a", "b", "return a * b");

var x = myFunction(4, 3);
```



大多数情况下，您可以避免在 JavaScript 中使用 new 关键词。



## 函数提升

在本教程中稍早前，您已经学到了“提升”（hoisting）。

Hoisting 是 JavaScript 将*声明*移动到当前作用域顶端的默认行为。

Hoisting 应用于变量声明和函数声明。

正因如此，JavaScript 函数能够在声明之前被调用：

```javascript
myFunction(5);

 function myFunction(y) {
     return y * y;
}
```

使用表达式定义的函数不会被提升。



## 自调用函数

函数表达式可以作为“自调用”。

自调用表达式是自动被调用（开始）的，在不进行调用的情况下。

函数表达式会自动执行，假如表达式后面跟着 ()。

您无法对函数声明进行自调用。

您需要在函数周围添加括号，以指示它是一个函数表达式：

### 实例

```javascript
(function () {
    var x = "Hello!!";      //我会调用我自己
})();
```



## 自调用函数

函数表达式可以作为“自调用”。

自调用表达式是自动被调用（开始）的，在不进行调用的情况下。

函数表达式会自动执行，假如表达式后面跟着 ()。

您无法对函数声明进行自调用。

您需要在函数周围添加括号，以指示它是一个函数表达式：

### 实例

```javascript
(function () {
    var x = "Hello!!";      //我会调用我自己
})();
```

上面的函数实际上是一个*匿名的自调用函数*（没有名称的函数）。



## 函数可用作值

JavaScript 函数可被用作值：

### 实例

```javascript
function myFunction(a, b) {
    return a * b;
}

var x = myFunction(4, 3);
```



JavaScript 函数可用在表达式中：

### 实例

```javascript
function myFunction(a, b) {
    return a * b;
}

var x = myFunction(4, 3) * 2;
```



## 函数是对象

JavaScript 中的 typeof 运算符会为函数返回 "function"。

但是最好是把 JavaScript 函数描述为对象。

JavaScript 函数都有*属性*和*方法*。

arguments.length 会返回函数被调用时收到的参数数目：

### 实例

```javascript
function myFunction(a, b) {
    return arguments.length;
}
```

toString() 方法以字符串返回函数：

### 实例

```javascript
function myFunction(a, b) {
    return a * b;
}

var txt = myFunction.toString();
```

定义为对象属性的函数，被称为对象的方法。

为创建新对象而设计的函数，被称为对象构造函数（对象构造器）。



## 箭头函数

箭头函数允许使用简短的语法来编写函数表达式。

您不需要 function 关键字、return 关键字和花括号。

### 实例

```javascript
// ES5
var x = function(x, y) {
  return x * y;
}

// ES6
const x = (x, y) => x * y;
```



箭头函数没有自己的 this。它们不适合定义对象方法。

箭头函数未被提升。它们必须在使用前进行定义。

使用 const 比使用 var 更安全，因为函数表达式始终是常量值。

如果函数是单个语句，则只能省略 return 关键字和大括号。因此，保留它们可能是一个好习惯：

### 实例

```javascript
const x = (x, y) => { return x * y };
```





# JavaScript 函数参数



**JavaScript 函数不会对参数值进行任何检查。**



## 函数参数

在本教程中稍早的时间，您已经学到了函数可以拥有*参数*：

```javascript
functionName(parameter1, parameter2, parameter3) {
    要执行的代码
}
```

*函数参数（parameter）*指的是在函数定义中列出的*名称*。

*函数参数（argument）*指的是传递到函数或由函数接收到的真实*值*。



## 参数规则

JavaScript 函数定义不会为参数（parameter）规定数据类型。

JavaScript 函数不会对所传递的参数（argument）实行类型检查。

JavaScript 函数不会检查所接收参数（argument）的数量。



## 参数默认

如果调用参数时*省略了参数*（少于被声明的数量），则丢失的值被设置为：*undefined*。

有时这是可以接受的，但是有时最好给参数指定默认值：

### 实例

```javascript
function myFunction(x, y) {
    if (y === undefined) {
          y = 0;
    } 
}
```



如果函数调用的*参数太多*（超过声明），则可以使用 *arguments 对象*来达到这些参数。

If a function is called with too many arguments (more than declared), these arguments can be reached using the arguments object.



## arguments 对象

JavaScript 函数有一个名为 arguments 对象的内置对象。

arguments 对象包含函数调用时使用的参数数组。

这样，您就可以简单地使用函数来查找（例如）数字列表中的最高值：

### 实例

```javascript
x = findMax(1, 123, 500, 115, 44, 88);

function findMax() {
    var i;
    var max = -Infinity;
    for (i = 0; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;
}
```



或创建一个函数来总和所有输入值：

### 实例

```javascript
x = sumAll(1, 123, 500, 115, 44, 88);

function sumAll() {
    var i, sum = 0;
    for (i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}
```



## 参数通过值传递

函数调用中的参数（parameter）是函数的参数（argument）。

JavaScript 参数通过*值*传递：函数只知道值，而不是参数的位置。

如果函数改变了参数的值，它不会改变参数的原始值。

参数的改变在函数之外是不可见的。



## 对象是由引用传递的

在 JavaScript 中，对象引用是值。

正因如此，对象的行为就像它们通过*引用*来传递：

如果函数改变了对象属性，它也改变了原始值。

对象属性的改变在函数之外是可见的。





# JavaScript 函数调用



**JavaScript 函数内部的代码会在“某物”调用它时执行。**



## 调用 JavaScript 函数

在函数被*定义*时，函数内部的代码不会执行。

在函数被*调用*时，函数内部的代码会被执行。

调用函数通常也可以说“启动函数”或“执行函数”。

在本教程中，我们使用“*调用*”。



## 以函数形式调用函数

### 实例

```javascript
function myFunction(a, b) {
    return a * b;
}
myFunction(10, 2);           // 将返回 20
```



以上函数不属于任何对象。但是在 JavaScript 中，始终存在一种默认的全局对象。

在 HTML 中，默认全局对象是 HTML 页面本身，所有上面的函数“属于”HTML 页面。

在浏览器中，这个页面对象就是浏览器窗口。上面的函数自动成为一个窗口函数。

myFunction() 和 window.myFunction() 是同一个函数：

### 实例

```javascript
function myFunction(a, b) {
    return a * b;
}
window.myFunction(10, 2);    // 也会返回 20
```

这是调用函数的常见方法，但并不是一个好习惯。

全局变量、方法或函数很容易在全局对象中产生命名冲突和漏洞。





## this 关键词

在 JavaScript 中，被称为 this 的事物，指的是“拥有”当前代码的对象。

this 的值，在函数中使用时，是“拥有”该函数的对象。

请注意 this 并不是变量。它属于关键词。您无法改变 this 的值。



## 全局对象

当不带拥有者对象调用对象时，this 的值成为全局对象。

在 web 浏览器中，全局对象就是浏览器对象。

本例以 this 的值返回这个 window 对象：

### 实例

```javascript
var x = myFunction();            // x 将成为 window 对象

function myFunction() {
   return this;
}
```

调用一个函数作为一个全局函数，会导致 this 的值成为全局对象。

作为变量来使用 window 对象很容易使程序崩溃。



## 作为方法来调用函数

在 JavaScript 中，您可以把函数定义为对象方法。

下面的例子创建了一个对象（myObject），带有两个属性（firstName 和 lastName），以及一个方法（fullName）：

### 实例

```javascript
var myObject = {
    firstName:"Bill",
    lastName: "Gates",
    fullName: function () {
        return this.firstName + " " + this.lastName;
    }
}
myObject.fullName();         // 将返回 "Bill Gates"
```

fullName 方法是一个函数。该函数属于对象。myObject 是函数的拥有者。

被称为 this 的事物，是“拥有”这段 JavaScript 代码的对象。在此例中，this 的值是 myObject。

测试一下！修改这个 fullName 方法来返回 this 的值：

### 实例

```javascript
var myObject = {
    firstName:"John",
    lastName: "Doe",
    fullName: function () {
        return this;
    }
}
myObject.fullName();          // 将返回 [object Object]（拥有者对象）
```

以对象方法来调用函数，会导致 this 的值成为对象本身。



## 通过函数构造器来调用函数

如果函数调用的前面是 new 关键字，那么这是一个构造函数调用。

它看起来像你创建一个新的函数，但由于 JavaScript 函数是对象，你实际上创建一个新对象：

### 实例

```javascript
// 这是函数构造器：
function myFunction(arg1, arg2) {
    this.firstName = arg1;
    this.lastName  = arg2;
}

// 创建了一个新对象：
var x = new myFunction("Bill", "Gates");
x.firstName;                             // 会返回 "Bill"
```

构造器调用会创建新对象。新对象会从其构造器继承属性和方法。

构造器内的 this 关键词没有值。

this 的值会成为调用函数时创建的新对象。





# JavaScript 函数 Call



## 方法重用

***使用 `call()` 方法，您可以编写能够在不同对象上使用的方法。***



## 函数是对象方法

在 JavaScript 中，函数是对象的方法。

如果一个函数不是 JavaScript 对象的方法，那么它就是全局对象的函数（参见前一章）。

下面的例子创建了带有三个属性的对象（firstName、lastName、fullName）。

### 实例

```javascript
var person = {
    firstName:"Bill",
    lastName: "Gates",
    fullName: function () {
        return this.firstName + " " + this.lastName;
    }
}
person.fullName();		// 将返回 "Bill Gates"
```

fullName 属性是一个*方法*。person 对象是该方法的*拥有者*。

fullName 属性属于 *person 对象的方法*。



## JavaScript call() 方法

call() 方法是预定义的 JavaScript 方法。

它可以用来调用所有者对象作为参数的方法。

通过 call()，您能够使用属于另一个对象的方法。

本例调用 person 的 fullName 方法，并用于 person1：

### 实例

```javascript
var person = {
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
var person1 = {
    firstName:"Bill",
    lastName: "Gates",
}
var person2 = {
    firstName:"Steve",
    lastName: "Jobs",
}
person.fullName.call(person1);  // 将返回 "Bill Gates"
```



本例调用 person 的 fullName 方法，并用于 person2：

### 实例

```javascript
var person = {
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
var person1 = {
    firstName:"John",
    lastName: "Doe",
}
var person2 = {
    firstName:"Mary",
    lastName: "Doe",
}
person.fullName.call(person2);  // 将返回 "Steve Jobs"
```



## 带参数的 call() 方法

call() 方法可接受参数：

### 实例

```javascript
var person = {
  fullName: function(city, country) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var person1 = {
  firstName:"Bill",
  lastName: "Gates"
}
person.fullName.call(person1, "Seattle", "USA");
```





# JavaScript 函数 Apply



## 方法重用

***通过 `apply() `方法，您能够编写用于不同对象的方法。***



## JavaScript apply() 方法

apply() 方法与 call() 方法非常相似：

在本例中，person 的 fullName 方法被*应用*到 person1：

### 实例

```javascript
var person = {
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
var person1 = {
    firstName: "Bill",
    lastName: "Gates",
}
person.fullName.apply(person1);  // 将返回 "Bill Gates"
```



## call() 和 apply() 之间的区别

不同之处是：

`call()` 方法分别接受参数。

`apply()` 方法接受数组形式的参数。

如果要使用数组而不是参数列表，则 `apply()` 方法非常方便。



## 带参数的 apply() 方法

apply() 方法接受数组中的参数：

### 实例

```javascript
var person = {
  fullName: function(city, country) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var person1 = {
  firstName:"John",
  lastName: "Doe"
}
person.fullName.apply(person1, ["Oslo", "Norway"]);
```



## 在数组上模拟 max 方法

您可以使用 Math.max() 方法找到（数字列表中的）最大数字：

### 实例

```javascript
Math.max(1,2,3);  // 会返回 3
```

由于 JavaScript 数组没有 max() 方法，因此您可以应用 Math.max() 方法。

### 实例(推崇)

```javascript
Math.max.apply(null, [1,2,3]); // 也会返回 3
```

第一个参数（null）无关紧要。在本例中未使用它。

这些例子会给出相同的结果：

### 实例

```javascript
Math.max.apply(Math, [1,2,3]); // 也会返回 3
```



## JavaScript 严格模式

在 JavaScript 严格模式下，如果 apply() 方法的第一个参数不是对象，则它将成为被调用函数的所有者（对象）。在“非严格”模式下，它成为全局对象。

