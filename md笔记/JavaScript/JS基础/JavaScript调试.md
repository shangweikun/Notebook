# JavaScript 调试



## console.log() 方法

如果您的浏览器支持调试，那么您可以使用 console.log() 在调试窗口中显示 JavaScript 的值：

### 实例

```html
<!DOCTYPE html>
<html>
<body>

<h1>My First Web Page</h1>

<script>
a = 5;
b = 6;
c = a + b;
console.log(c);
</script>

</body>
</html>
```

**提示：**请访问我们的 JavaScript Console 参考手册，获取更多有关 console.log() 方法的信息。



## 设置断点

在调试窗口中，您可在 JavaScript 代码中设置断点。

在每个断点中，JavaScript 将停止执行，以使您能够检查 JavaScript 的值。

在检查值之后，您可以恢复代码执行。



## debugger 关键词

*debugger* 关键词会停止 JavaScript 的执行，并调用（如果有）调试函数。

这与在调试器中设置断点的功能是一样的。

如果调试器不可用，debugger 语句没有效果。

如果调试器已打开，此代码会在执行第三行之前停止运行。

### 实例

```javascript
var x = 15 * 5;
debugger;
document.getElementbyId("demo").innerHTML = x; 
```





# JavaScript 常见错误