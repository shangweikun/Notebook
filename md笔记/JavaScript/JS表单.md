# JavaScript 表单



## JavaScript 表单验证

HTML 表单验证能够通过 JavaScript 来完成。

```html
<!DOCTYPE html>
<html>
<head>
<script>
function validateForm() {
  var x = document.forms["myForm"]["fname"].value;
  if (x == "") {
    alert("必须填写姓名！");
    return false;
  }
}
</script>
</head>
<body>

<form name="myForm" action="/demo/action_page.php" onsubmit="return validateForm()" method="post">
  姓名：<input type="text" name="fname">
  <input type="submit" value="提交">
</form>

</body>
</html>
```



## JavaScript 能够验证数字输入

JavaScript 常用于验证数字输入：

```html
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript 能够验证输入</h2>

<p>请输入 1 与 10 之间的数：</p>

<input id="numb">

<button type="button" onclick="myFunction()">提交</button>

<p id="demo"></p>

<script>
function myFunction() {
  var x, text;

  // 获取 id="numb" 的输入字段的值
  x = document.getElementById("numb").value;

  // 如果 x 不是数字或小于 1 或大于 10
  if (isNaN(x) || x < 1 || x > 10) {
    text = "输入无效";
  } else {
    text = "输入有效";
  }
  document.getElementById("demo").innerHTML = text;
}
</script>

</body>
</html>
```



## 自动 HTML 表单验证

HTML 表单验证能够被浏览器自动执行：

如果表单字段（fname）是空的，*required* 属性防止表单被提交：

### HTML 表单实例

```html
<form action="/action_page_post.php" method="post">
   <input type="text" name="fname" required>
   <input type="submit" value="Submit">
</form>
```



## HTML 约束验证

HTML5 引入了一种新的 HTML 验证概念，名为*约束验证（constraint validation）*。

HTML 约束验证基于：

- 约束验证 *HTML 输入属性*
- 约束验证 *CSS 伪选择器*
- 约束验证 *DOM 属性和方法*

## 约束验证 HTML 输入属性

| 属性     | 描述                      |
| :------- | :------------------------ |
| disabled | 规定 input 元素应该被禁用 |
| max      | 规定 input 元素的最大值   |
| min      | 规定 input 元素的最小值   |
| pattern  | 规定 input 元素的值模式   |
| required | 规定输入字段需要某个元素  |
| type     | 规定 input 元素的类型     |



## 约束验证 CSS 伪选择器

| 选择器    | 描述                                      |
| :-------- | :---------------------------------------- |
| :disabled | 选择设置了 "disabled" 属性的 input 元素。 |
| :invalid  | 选择带有无效值的 input 元素。             |
| :optional | 选择未设置 "required" 属性的 input 元素。 |
| :required | 选择设置了 "required" 属性的 input 元素。 |
| :valid    | 选择带有有效值的 input 元素。             |





# JavaScript 验证 API



## 约束验证 DOM 方法

| 属性                | 描述                                       |
| :------------------ | :----------------------------------------- |
| checkValidity()     | 返回 true，如果 input 元素包含有效数据     |
| setCustomValidity() | 设置 input 元素的 validationMessage 属性。 |



## checkValidity() 方法

如果输入字段包含无效的数据，显示一条消息：

```html
<!DOCTYPE html>
<html>
<body>

<p>输入数字并点击提交：</p>

<input id="id1" type="number" min="100" max="300" required>

<button onclick="myFunction()">提交</button>

<p>如果该数字小于 100 或大于 300，将显示错误消息。</p>

<p id="demo"></p>

<script>
function myFunction() {
  var inpObj = document.getElementById("id1");
  inpObj.setCustomValidity("生效");
  if (!inpObj.checkValidity()) {
    document.getElementById("demo").innerHTML = inpObj.validationMessage;
  } else {
    document.getElementById("demo").innerHTML = "输入有效";
  } 
} 
</script>

</body>
</html>
```



## 约束验证 DOM 属性

| 属性              | 描述                                          |
| :---------------- | :-------------------------------------------- |
| validity          | 包含与 input 元素的合法性相关的布尔属性。     |
| validationMessage | 包含当 validity 为 false 时浏览器显示的消息。 |
| willValidate      | 指示是否验证 input 元素。                     |



## 合法性属性

input 元素的 validity 属性包含了与数据合法性相关的一系列属性：

| 属性            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| customError     | 设置为 true，如果设置自定义的合法性消息。                    |
| patternMismatch | 设置为 true，如果元素值不匹配其 pattern 属性。               |
| rangeOverflow   | 设置为 true，如果元素值大约其 max 属性。                     |
| rangeUnderflow  | 设置为 true，如果元素值小于其 min 属性。                     |
| stepMismatch    | 当字段拥有 step 属性，且输入的 value 值不符合设定的间隔值时，该属性值为 true。 |
| tooLong         | 设置为 true，如果元素值超过了其 maxLength 属性。             |
| typeMismatch    | 当字段的 type 是 email 或者 url 但输入的值不是正确的类型时，属性值为 true。 |
| valueMissing    | 设置为 true，如果元素（包含 required）没有值。               |
| valid           | 设置为 true，如果元素值是有效的。                            |

实例

如果输入字段中的数字大于 100（input 元素的 max 属性），则显示一条消息：

### rangeOverflow 属性

```html
<input id="id1" type="number" max="100">
<button onclick="myFunction()">OK</button>

<p id="demo"></p>

<script>
function myFunction() {
    var txt = "";
    if (document.getElementById("id1").validity.rangeOverflow) {
        txt = "值太大";
      
    }
     document.getElementById("demo").innerHTML = txt;
}
</script> 
```

如果输入字段中的数字小于 100（input 元素的 min 属性），则显示一条消息：

### rangeUnderflow 属性

```html
<input id="id1" type="number" min="100">
<button onclick="myFunction()">OK</button>

<p id="demo"></p>

<script>
function myFunction() {
     var txt = "";
    if (document.getElementById("id1").validity.rangeUnderflow) {
        txt = "值太小";
    }
     document.getElementById("demo").innerHTML = txt;
}
</script>
```

