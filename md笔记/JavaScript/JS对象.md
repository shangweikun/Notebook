# JavaScript 对象定义

在 JavaScript 中，几乎“所有事物”都是对象。

- 布尔是对象（如果用 *new* 关键词定义）
- 数字是对象（如果用 *new* 关键词定义）
- 字符串是对象（如果用 *new* 关键词定义）
- 日期永远都是对象
- 算术永远都是对象
- 正则表达式永远都是对象
- 数组永远都是对象
- 函数永远都是对象
- 对象永远都是对象

所有 JavaScript 值，除了原始值，都是对象。



## JavaScript 原始值

*原始值*指的是没有属性或方法的值。

*原始数据类型*指的是拥有原始值的数据。

JavaScript 定义了 5 种原始数据类型：

- string
- number
- boolean
- null
- undefined

原始值是一成不变的（它们是硬编码的，因此不能改变）。

假设 x = 3.14，您能够改变 x 的值。但是您无法改变 3.14 的值。

| 值        | 类型      | 注释                       |
| :-------- | :-------- | :------------------------- |
| "Hello"   | string    | "Hello" 始终是 "Hello"     |
| 3.14      | number    | 3.14 始终是 3.14           |
| true      | boolean   | true 始终是 true           |
| false     | boolean   | false 始终是 false         |
| null      | null      | (object) null 始终是 null  |
| undefined | undefined | undefined 始终是 undefined |



## 对象是包含变量的变量

JavaScript 变量能够包含单个的值：

实例

```javascript
var person = "Bill Gates";
```

对象也是变量。但是对象能够包含很多值。

值按照*名称 : 值*对的形式编写（名称和值以冒号分隔）。

实例

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
```



## 对象属性

JavaScript 对象中的命名值，被称为*属性*。

| 属性      | 值    |
| :-------- | :---- |
| firstName | Bill  |
| lastName  | Gates |
| age       | 62    |
| eyeColor  | blue  |

以名称值对书写的对象类似于：

- PHP 中的关联数组
- Python 中的字典
- C 中的哈希表
- Java 中的哈希映射
- Ruby 和 Perl 中的散列



## 对象方法

方法是可以在对象上执行的*动作*。

对象属性可以是原始值、其他对象以及函数。

*对象方法*是包含*函数定义*的对象属性。

| 属性      | 值                                                        |
| :-------- | :-------------------------------------------------------- |
| firstName | Bill                                                      |
| lastName  | Gates                                                     |
| age       | 62                                                        |
| eyeColor  | blue                                                      |
| fullName  | function() {return this.firstName + " " + this.lastName;} |

<strong>JavaScript 对象是被称为属性和方法的命名值的容器。</strong>



## 创建 JavaScript 对象

通过 JavaScript，您能够定义和创建自己的对象。

有不同的方法来创建对象：

- 定义和创建单个对象，使用对象文字。
- 定义和创建单个对象，通过关键词 new。
- 定义对象构造器，然后创建构造类型的对象。

在 ECMAScript 5 中，也可以通过函数 Object.create() 来创建对象。



## 使用 JavaScript 关键词 new

下面的例子也创建了带有四个属性的新的 JavaScript 对象：

实例

```
var person = new Object();
person.firstName = "Bill";
person.lastName = "Gates";
person.age = 50;
person.eyeColor = "blue"; 
```



## JavaScript 对象是易变的

对象是易变的：它们通过引用来寻址，而非值。

如果 person 是一个对象，下面的语句不会创建 person 的副本：

```javascript
var x = person;  // 这不会创建 person 的副本。
```

对象 x *并非* person 的副本。它*就是* person。x 和 person 是同一个对象。

对 x 的任何改变都将改变 person，因为 x 和 person 是相同的对象。

实例

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"}
 
var x = person;
x.age = 10;           // 这将同时改变 both x.age 和 person.age
```



**注释：**JavaScript 变量不是易变的。只有 JavaScript 对象如此。

https://www.w3school.com.cn/js/js_object_definition.asp





# JavaScript 对象属性

**属性是任何 JavaScript 对象最重要的部分。**



## JavaScript 属性

属性指的是与 JavaScript 对象相关的值。

JavaScript 对象是无序属性的集合。

属性通常可以被修改、添加和删除，但是某些属性是只读的。

## 访问 JavaScript 属性

访问对象属性的语法是：

```javascript
objectName.property           // person.age
```

或者：

```javascript
objectName["property"]       // person["age"]
```

或者：

```javascript
objectName[expression]       // x = "age"; person[x]
```



## JavaScript for...in 循环

JavaScript for...in 语句遍历对象的属性。

### 语法

```
for (variable in object) {
    要执行的代码
}
```

for...in 循环中的代码块会为每个属性执行一次。

循环对象的属性：

实例

```javascript
var person = {fname:"Bill", lname:"Gates", age:62}; 

for (x in person) {
    txt += person[x];
}
```





## 添加新属性

您可以通过简单的赋值，向已存在的对象添加新属性。

假设 person 对象已存在 - 那么您可以为其添加新属性：

实例

```javascript
person.nationality = "English";
```



## 删除属性

delete 关键词从对象中删除属性：

实例

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
delete person.age;   // 或 delete person["age"];
```



`delete` 关键词会同时删除属性的值和属性本身。

删除完成后，属性在被添加回来之前是无法使用的。

`delete` 操作符被设计用于对象属性。它对变量或函数没有影响。

`delete` 操作符不应被用于预定义的 JavaScript 对象属性。这样做会使应用程序崩溃。

## 原型属性

JavaScript 对象继承了它们的原型的属性。

`delete` 关键词不会删除被继承的属性，但是如果您删除了某个原型属性，则将影响到所有从原型继承的对象。



# JavaScript 对象方法

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  id       : 648,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
```



## JavaScript 方法

JavaScript 方法是能够在对象上执行的动作。

JavaScript *方法*是包含*函数定义*的属性。

| 属性      | 值                                                        |
| :-------- | :-------------------------------------------------------- |
| firstName | Bill                                                      |
| lastName  | Gates                                                     |
| age       | 62                                                        |
| eyeColor  | blue                                                      |
| fullName  | function() {return this.firstName + " " + this.lastName;} |



## *this* 关键词

在 JavaScript 中，被称为 this 的事物，指的是拥有该 JavaScript 代码的对象。

this 的值，在函数中使用时，是“拥有”该函数的对象。

请注意 this 并非变量。它是关键词。您无法改变 this 的值。



## 访问对象方法

请使用如下语法创建对象方法：

```
methodName : function() { 代码行 }
```

请通过如下语法来访问对象方法：

```
objectName.methodName()
```

您通常会把 fullName() 描述为 person 对象的方法，把 fullName 描述为属性。

fullName 属性在被通过 () 调用后会以函数形式执行。

此例访问 person 对象的 fullName() *方法*：

实例

```javascript
name = person.fullName();
```



如果您访问 fullName *属性*时没有使用 ()，则将返回*函数定义*：

实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 对象方法</h1>

<p>如果您不使用 () 访问对象，则返回函数定义：</p>

<p id="demo"></p>

<script>
// 创建对象：
var person = {
    firstName: "Bill",
    lastName : "Gates",
    id       : 12345,
    fullName : function() {
       return this.firstName + " " + this.lastName;
    }
};

// 显示对象中的数据：
document.getElementById("demo").innerHTML = person.fullName;
</script>

</body>
</html>
```



## 添加新的方法

向对象添加方法是在构造器函数内部完成的：

```html
<!DOCTYPE html>
<html>
<body>

<p id="demo"></p>

<script>
var person =new person("小","李",21,"red");
person.changeName("王");
person.name = function() {
  return this.firstName + " " + this.lastName;
};
document.getElementById("demo").innerHTML =
"My friend is " + person.name(); 

function person(firstName, lastName, age, eyeColor) {
    this.firstName = firstName;  
    this.lastName = lastName;
    this.age = age;
    this.eyeColor = eyeColor;
    this.changeName = function (name) {
        this.lastName = name;
    };
}
</script>

</body>
</html>
```



# JavaScript 对象访问器



## JavaScript Getter（get 关键词）

本例使用 lang 属性来获取 language 属性的值。

实例

```javascript
// 创建对象：
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "en",
  get lang() {
    return this.language;
  }
};

// 使用 getter 来显示来自对象的数据：
document.getElementById("demo").innerHTML = person.lang;
```



## JavaScript Setter（set 关键词）

本例使用 lang 属性来设置 language 属性的值。

实例

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "",
  set lang(lang) {
    this.language = lang;
  }
};

// 使用 setter 来设置对象属性：
person.lang = "en";

// 显示来自对象的数据：
document.getElementById("demo").innerHTML = person.language;
```



## JavaScript 函数还是 Getter？

下面两个例子的区别在哪里？

###例子 1

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};

// 使用方法来显示来自对象的数据：
document.getElementById("demo").innerHTML = person.fullName();
```

### 例子 2

```javascript
var person = {
  firstName: "Bill",
  lastName : "Gates",
  get fullName() {
    return this.firstName + " " + this.lastName;
  }
};

// 使用 getter 来显示来自对象的数据：
document.getElementById("demo").innerHTML = person.fullName;
```



## 为什么使用 Getter 和 Setter？

- 它提供了更简洁的语法
- 它允许属性和方法的语法相同
- 它可以确保更好的数据质量
- 有利于后台工作



## 一个计数器实例

### 实例

```javascript
var obj = {
  counter : 0,
  get reset() {
    this.counter = 0;
  },
  get increment() {
    this.counter++;
  },
  get decrement() {
    this.counter--;
  },
  set add(value) {
    this.counter += value;
  },
  set subtract(value) {
    this.counter -= value;
  }
};

// 操作计数器：
obj.reset;
obj.add = 5;
obj.subtract = 1;
obj.increment;
obj.decrement;
Object.defineProperty()
```



Object.defineProperty() 方法也可用于添加 Getter 和 Setter：

### 实例

```javascript
// 定义对象
var obj = {counter : 0};

// 定义 setters
Object.defineProperty(obj, "reset", {
  get : function () {this.counter = 0;}
});
Object.defineProperty(obj, "increment", {
  get : function () {this.counter++;}
});
Object.defineProperty(obj, "decrement", {
  get : function () {this.counter--;}
});
Object.defineProperty(obj, "add", {
  set : function (value) {this.counter += value;}
});
Object.defineProperty(obj, "subtract", {
  set : function (value) {this.counter -= value;}
});

// 操作计数器：
obj.reset;
obj.add = 5;
obj.subtract = 1;
obj.increment;
obj.decrement;
```





# JavaScript 对象构造器

```javascript
function Person(first, last, age, eye) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eye;
}
```



## 对象类型（蓝图）（类）

前一章的实例是有限制的。它们只创建单一对象。

有时我们需要创建相同“类型”的许多对象的“*蓝图*”。

创建一种“对象类型”的方法，是使用*对象构造器函数*。

在上面的例子中，*函数 Person()* 就是对象构造器函数。

通过 *new* 关键词调用构造器函数可以创建相同类型的对象：

```javascript
var myFather = new Person("Bill", "Gates", 62, "blue");
var myMother = new Person("Steve", "Jobs", 56, "green");
```



## 为构造器添加属性

与向已有对象添加新属性不同，您无法为对象构造器添加新属性：

实例

```javascript
Person.nationality = "English";
```



**PS：为对象添加属性或属性方法，其添加目标是已有对象，而非对象构造器；**





## 内建 JavaScript 构造器

JavaScript 提供用于原始对象的构造器：

实例

```javascript
var x1 = new Object();    // 一个新的 Object 对象
var x2 = new String();    // 一个新的 String 对象
var x3 = new Number();    // 一个新的 Number 对象
var x4 = new Boolean();   // 一个新的 Boolean 对象
var x5 = new Array();     // 一个新的 Array 对象
var x6 = new RegExp();    // 一个新的 RegExp 对象
var x7 = new Function();  // 一个新的 Function 对象
var x8 = new Date();      // 一个新的 Date 对象
```



## 您知道吗？

正如以上所见，JavaScript 提供原始数据类型字符串、数字和布尔的对象版本。但是并无理由创建复杂的对象。原始值快得多！

请使用对象字面量 {} 代替 new Object()。

请使用字符串字面量 "" 代替 new String()。

请使用数值字面量代替 Number()。

请使用布尔字面量代替 new Boolean()。

请使用数组字面量 [] 代替 new Array()。

请使用模式字面量代替 new RexExp()。

请使用函数表达式 () {} 代替 new Function()。

实例

```javascript
var x1 = {};            // 新对象
var x2 = "";            // 新的原始字符串
var x3 = 0;             // 新的原始数值
var x4 = false;         // 新的原始逻辑值
var x5 = [];            // 新的数组对象
var x6 = /()/           // 新的正则表达式对象
var x7 = function(){};  // 新的函数对象
```





# JavaScript 对象原型

**所有 JavaScript 对象都从原型继承属性和方法。**



## 原型继承

所有 JavaScript 对象都从原型继承属性和方法。

日期对象继承自 Date.prototype。数组对象继承自 Array.prototype。Person 对象继承自 Person.prototype。

Object.prototype 位于原型继承链的顶端：

日期对象、数组对象和 Person 对象都继承自 Object.prototype。



## 向对象添加属性和方法

有时，您希望向所有给定类型的已有对象添加新属性（或方法）。

有时，您希望向对象构造器添加新属性（或方法）。



## 使用 prototype 属性

JavaScript prototype 属性允许您为**对象构造器**添加新属性：

```javascript
function Person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
}
Person.prototype.nationality = "English";
```



# JavaScript ES5 对象方法

## ES5 新的对象方法

```javascript
// 添加或更改对象属性
Object.defineProperty(object, property, descriptor)

// 添加或更改多个对象属性
Object.defineProperties(object, descriptors)

// 访问属性
Object.getOwnPropertyDescriptor(object, property)

// 以数组返回所有属性
Object.getOwnPropertyNames(object)

// 以数组返回所有可枚举的属性
Object.keys(object)

// 访问原型
Object.getPrototypeOf(object)

// 阻止向对象添加属性
Object.preventExtensions(object)

// 如果可将属性添加到对象，则返回 true
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



## 更改元数据

ES5 允许更改以下属性元数据：

```javascript
writable : true      // 属性值可修改
enumerable : true    // 属性可枚举
configurable : true  // 属性可重新配置
writable : false     // 属性值不可修改
enumerable : false   // 属性不可枚举
configurable : false // 属性不可重新配置
```

ES5 允许更改 getter 和 setter：

```javascript
// 定义 getter
get: function() { return language }
// 定义 setter
set: function(value) { language = value }
```

此例使语言为只读：

```javascript
Object.defineProperty(person, "language", {writable:false});
```

此例使语言不可枚举：

```javascript
Object.defineProperty(person, "language", {enumerable:false});
```



## 添加 Getter 和 Setter

Object.defineProperty() 方法也可以用于添加 Getter 和 Setter：

### 实例

```javascript
// 创建对象
var person = {firstName:"Bill", lastName:"Gates"};

// 定义 getter
Object.defineProperty(person, "fullName", {
  get : function () {return this.firstName + " " + this.lastName;}
});
```

https://www.w3school.com.cn/js/js_object_es5.asp

