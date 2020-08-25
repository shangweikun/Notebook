# JavaScript  对象



## Math

###Math.round()

Math.round(x) 的返回值是 x 四舍五入为最接近的整数：

###Math.pow()

Math.pow(x, y) 的返回值是 x 的 y 次幂：

###Math.sqrt()

Math.sqrt(x) 返回 x 的平方根：

###Math.abs()

Math.abs(x) 返回 x 的绝对（正）值：

###Math.ceil()

Math.ceil(x) 的返回值是 x *上舍入*最接近的整数：

###Math.floor()

Math.floor(x) 的返回值是 x *下舍入*最接近的整数：

###Math.sin()

Math.sin(x) 返回角 x（以弧度计）的正弦（介于 -1 与 1 之间的值）。

###Math.cos()

Math.cos(x) 返回角 x（以弧度计）的余弦（介于 -1 与 1 之间的值）。

如果您希望使用角度替代弧度，则需要将角度转换为弧度：

###Math.min() 和 Math.max()

Math.min() 和 Math.max() 可用于查找参数列表中的最低或最高值：

###Math.random()

Math.random() 返回介于 0（包括） 与 1（不包括） 之间的随机数：

###Math 属性（常量）

JavaScript 提供了可由 Math 对象访问的 8 个数学常量：

实例

```
Math.E          // 返回欧拉指数（Euler's number）
Math.PI         // 返回圆周率（PI）
Math.SQRT2      // 返回 2 的平方根
Math.SQRT1_2    // 返回 1/2 的平方根
Math.LN2        // 返回 2 的自然对数
Math.LN10       // 返回 10 的自然对数
Math.LOG2E      // 返回以 2 为底的 e 的对数（约等于 1.414）
Math.LOG10E     // 返回以 10 为底的 e 的对数（约等于0.434）
```

###Math 对象方法

| 方法             | 描述                                                     |
| :--------------- | :------------------------------------------------------- |
| abs(x)           | 返回 x 的绝对值                                          |
| acos(x)          | 返回 x 的反余弦值，以弧度计                              |
| asin(x)          | 返回 x 的反正弦值，以弧度计                              |
| atan(x)          | 以介于 -PI/2 与 PI/2 弧度之间的数值来返回 x 的反正切值。 |
| atan2(y,x)       | 返回从 x 轴到点 (x,y) 的角度                             |
| ceil(x)          | 对 x 进行上舍入                                          |
| cos(x)           | 返回 x 的余弦                                            |
| exp(x)           | 返回 Ex 的值                                             |
| floor(x)         | 对 x 进行下舍入                                          |
| log(x)           | 返回 x 的自然对数（底为e）                               |
| max(x,y,z,...,n) | 返回最高值                                               |
| min(x,y,z,...,n) | 返回最低值                                               |
| pow(x,y)         | 返回 x 的 y 次幂                                         |
| random()         | 返回 0 ~ 1 之间的随机数                                  |
| round(x)         | 把 x 四舍五入为最接近的整数                              |
| sin(x)           | 返回 x（x 以角度计）的正弦                               |
| sqrt(x)          | 返回 x 的平方根                                          |
| tan(x)           | 返回角的正切                                             |

更多：https://www.w3school.com.cn/jsref/jsref_obj_math.asp



###Math.random()

Math.random() 返回 0（包括） 至 1（不包括） 之间的随机数：



## JavaScript 随机整数

Math.random() 与 Math.floor() 一起使用用于返回随机整数。

实例

```javascript
Math.floor(Math.random() * 10);		// 返回 0 至 9 之间的数
```

###一个适当的随机函数

正如你从上面的例子看到的，创建一个随机函数用于生成所有随机整数是一个好主意。

这个 JavaScript 函数始终返回介于 min（包括）和 max（不包括）之间的随机数：

实例

```
function getRndInteger(min, max) {
    return Math.floor(Math.random() * (max - min) ) + min;
}
```





#JavaScript 逻辑

## Boolean() 函数

您可以使用 Boolean() 函数来确定表达式（或变量）是否为真：

实例

```javascript
Boolean(10 > 9)        // 返回 true
```



## 布尔可以是对象

通常 JavaScript 布尔是由字面量创建的原始值：

```
var x = false
```

但是布尔也可以通过关键词 new 作为对象来定义：

```
var y = new Boolean(false)
```



## 比较运算符

比较运算符在逻辑语句中使用，以判定变量或值是否相等。

我们给定 x = 5，下表中解释了比较运算符：

| 运算符 | 描述                 | 比较      | 返回  | 测试                                                         |
| :----- | :------------------- | :-------- | :---- | :----------------------------------------------------------- |
| ==     | 等于                 | x == 8    | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_01) |
|        |                      | x == 5    | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_02) |
|        |                      | x == "5"  | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_03) |
| ===    | 值相等并且类型相等   | x === 5   | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_04) |
|        |                      | x === "5" | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_05) |
| !=     | 不相等               | x != 8    | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_06) |
| !==    | 值不相等或类型不相等 | x !== 5   | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_07) |
|        |                      | x !== "5" | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_08) |
|        |                      | x !== 8   | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_09) |
| >      | 大于                 | x > 8     | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_10) |
| <      | 小于                 | x < 8     | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_11) |
| >=     | 大于或等于           | x >= 8    | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_12) |
| <=     | 小于或等于           | x <= 8    | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_13) |



## 比较不同的类型

比较不同类型的数据也许会出现不可预料的结果。

如果将字符串与数字进行比较，那么在做比较时 JavaScript 会把字符串转换为数值。空字符串将被转换为 0。非数值字符串将被转换为始终为 false 的 NaN。

| 案例        | 值    | 测试                                                         |
| :---------- | :---- | :----------------------------------------------------------- |
| 2 < 12      | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_18) |
| 2 < "12"    | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_19) |
| 2 < "John"  | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_20) |
| 2 > "John"  | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_21) |
| 2 == "John" | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_22) |
| "2" < "12"  | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_23) |
| "2" > "12"  | true  | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_24) |
| "2" == "12" | false | [试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_comparison_25) |



# JavaScript Switch 语句

**switch 语句用于基于不同条件执行不同动作。**





# JavaScript 循环

## 不同类型的循环

JavaScript 支持不同类型的循环：

- for - 多次遍历代码块
- for/in - 遍历对象属性
- while - 当指定条件为 true 时循环一段代码块
- do/while - 当指定条件为 true 时循环一段代码块



## While 循环

while 循环会一直循环代码块，只要指定的条件为 true。

### 语法

```
while (条件) {
    要执行的代码块
}
```



## Do/While 循环

do/while 循环是 while 循环的变体。在检查条件是否为真之前，这种循环会执行一次代码块，然后只要条件为真就会重复循环。

### 语法

```
do {
    要执行的代码块
}

while (条件);
```



**break 语句“跳出”循环。**

**continue 语句“跳过”循环中的一个迭代。**



# JavaScript 类型转换

**Number() 转换数值，String() 转换字符串，Boolean() 转换布尔值。**

## JavaScript 数据类型

JavaScript 中有五种可包含值的数据类型：

- 字符串（string）
- 数字（number）
- 布尔（boolean）
- 对象（object）
- 函数（function）

有三种对象类型：

- 对象（Object）
- 日期（Date）
- 数组（Array）

同时有两种不能包含值的数据类型：

- null
- undefined





## constructor 属性

constructor 属性返回所有 JavaScript 变量的构造器函数。

### 实例

```javascript
"Bill".constructor                 // 返回 "function String()  { [native code] }"
(3.14).constructor                 // 返回 "function Number()  { [native code] }"
false.constructor                  // 返回 "function Boolean() { [native code] }"
[1,2,3,4].constructor              // 返回 "function Array()   { [native code] }"
{name:'Bill', age:62}.constructor  // 返回" function Object()  { [native code] }"
new Date().constructor             // 返回 "function Date()    { [native code] }"
function () {}.constructor         // 返回 "function Function(){ [native code] }"
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=js_type_convert_constructor_all)

您可以通过检查 constructor 属性来确定某个对象是否为数组（包含单词 "Array"）：

实例

```javascript
function isArray(myArray) {
    return myArray.constructor.toString().indexOf("Array") > -1;
}
```

或者更简单，您可以检查对象是否是数组函数：

实例

```javascript
function isArray(myArray) {
    return myArray.constructor === Array;
}
```



在[数字方法](https://www.w3school.com.cn/js/js_number_methods.asp)这一章，您将学到更多可用于将数值转换为字符串的方法：

| 方法            | 描述                                                   |
| :-------------- | :----------------------------------------------------- |
| toExponential() | 返回字符串，对数字进行舍入，并使用指数计数法来写。     |
| toFixed()       | 返回字符串，对数字进行舍入，并使用指定位数的小数来写。 |
| toPrecision()   | 返回字符串，把数字写为指定的长度。                     |



在[日期方法](https://www.w3school.com.cn/js/js_date_methods.asp)这一章，您能够找到更多可用于把日期转换为字符串的方法：

| 方法              | 描述                                    |
| :---------------- | :-------------------------------------- |
| getDate()         | 获得以数值计（1-31）的日                |
| getDay()          | 或者以数值计（0-6）的周                 |
| getFullYear()     | 获得四位的年（yyyy）                    |
| getHours()        | 获得时（0-23）                          |
| getMilliseconds() | 获得毫秒（0-999）                       |
| getMinutes()      | 获得分钟（0-59）                        |
| getMonth()        | 获得月（0-11）                          |
| getSeconds()      | 获得秒（0-59）                          |
| getTime()         | 获得时间（1970 年 1 月 1 日以来的毫秒） |



在[数字方法](https://www.w3school.com.cn/js/js_number_methods.asp)这一章中，您将找到更多可用于把字符串转换为数字的方法：

| 方法         | 描述                     |
| :----------- | :----------------------- |
| parseFloat() | 解析字符串并返回浮点数。 |
| parseInt()   | 解析字符串并返回整数。   |



## 一元 + 运算符

*一元的 + 运算符*可用于把变量转换为数字：

实例

```javascript
var y = "5";      // y 是字符串
var x = + y;      // x 是数字
```

如果无法转换变量，则仍会成为数字，但是值为 NaN（Not a number）：

实例

```javascript
var y = "Bill";   // y 是字符串
var x = + y;      // x 是数字 (NaN)
```

连接：https://www.w3school.com.cn/js/js_type_conversion.asp



# JavaScript 位运算符

## JavaScript 位运算符

| 运算符 | 名称         | 描述                                                     |
| :----- | :----------- | :------------------------------------------------------- |
| &      | AND          | 如果两位都是 1 则设置每位为 1                            |
| \|     | OR           | 如果两位之一为 1 则设置每位为 1                          |
| ^      | XOR          | 如果两位只有一位为 1 则设置每位为 1                      |
| ~      | NOT          | 反转所有位                                               |
| <<     | 零填充左位移 | 通过从右推入零向左位移，并使最左边的位脱落。             |
| >>     | 有符号右位移 | 通过从左推入最左位的拷贝来向右位移，并使最右边的位脱落。 |
| >>>    | 零填充右位移 | 通过从左推入零来向右位移，并使最右边的位脱落。           |

### 实例

| 操作    | 结果 | 等同于       | 结果 |
| :------ | :--- | :----------- | :--- |
| 5 & 1   | 1    | 0101 & 0001  | 0001 |
| 5 \| 1  | 5    | 0101 \| 0001 | 0101 |
| 5 ^ 1   | 4    | 0101 ^ 0001  | 0100 |
| ~ 5     | 10   | ~0101        | 1010 |
| 5 << 1  | 10   | 0101 << 1    | 1010 |
| 5 >> 1  | 2    | 0101 >> 1    | 0010 |
| 5 >>> 1 | 2    | 0101 >>> 1   | 0010 |



## JavaScript 使用 32 位按位运算数

JavaScript 将数字存储为 64 位浮点数，但所有按位运算都以 32 位二进制数执行。

在执行位运算之前，JavaScript 将数字转换为 32 位有符号整数。

执行按位操作后，结果将转换回 64 位 JavaScript 数。

上面的例子使用 4 位无符号二进制数。所以 ~ 5 返回 10。

由于 JavaScript 使用 32 位有符号整数，JavaScript 将返回 -6。

00000000000000000000000000000101 (5)

11111111111111111111111111111010 (~5 = -6)

有符号整数使用最左边的位作为减号。

### PS

IEEE 754的浮点数规范中，负数采用 ***补码*** 表示





## 把十进制转换为二进制

实例

```javascript
function dec2bin(dec){
    return (dec >>> 0).toString(2);
}
```

## 把二进制转换为十进制

实例

```javascript
function bin2dec(bin){
    return parseInt(bin, 2).toString(10);
}
```

