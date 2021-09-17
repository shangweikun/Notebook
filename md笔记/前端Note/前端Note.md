

##XHTML

***最重要***

https://www.w3school.com.cn/xhtml/xhtml_structural_01.asp

https://www.w3school.com.cn/xhtml/xhtml_structural_02.asp



XHTML和HTML的区别 - ***更规范***

- XHTML 元素必须被正确地嵌套。
- XHTML 元素必须被关闭。
- 标签名必须用小写字母。
- XHTML 文档必须拥有根元素。



最小化文档

```xhtml
<!DOCTYPE Doctype goes here>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>Title goes here</title>
</head>

<body>
</body>

</html>
```



### 存在三种XHTML文档类型：

- STRICT（严格类型）
- TRANSITIONAL（过渡类型）
- FRAMESET（框架类型）

```xhtml
<!DOCTYPE html
PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```



```xhtml
<hr> 和 <br> 标签应该被替换为 <hr /> 和 <br />
```



W3C 已将 XHTML 的定义分为28种模型：

| 模块名称                                      | 描述                                            |
| :-------------------------------------------- | :---------------------------------------------- |
| Applet Module (Applet模块)                    | 定义已被废弃的applet元素。                      |
| Base Module (基础模块)                        | 定义基本元素。                                  |
| Basic Forms Module (基础表单模块)             | 定义基本的表单元素 (forms)。                    |
| Basic Tables Module (基础表格模块)            | 定义基本的表格元素 (table)。                    |
| Bi-directional Text Module (双向文本模块)     | 定义bdo元素。                                   |
| Client Image Map Module(客户端图像映射模块)   | 定义浏览器端图像映射元素(image map elements)。  |
| Edit Module (编辑模块)                        | 定义编辑元素删除和插入。                        |
| Forms Module (表单模块)                       | 定义所有在表单中使用的元素。                    |
| Frames Module (框架模块)                      | 定义frameset元素。                              |
| Hypertext Module (超文本模块)                 | 定义a元素。                                     |
| Iframe Module (内联框架模块)                  | 定义iframe元素。                                |
| Image Module (图像模块)                       | 定义图像元素 (img)。                            |
| Intrinsic Events Module ()                    | 定义事件属性 (event)，比如onblur和onchange。    |
| Legacy Module (遗留模块)                      | 定义被废弃的元素和属性。                        |
| Link Module (链接模块)                        | 定义链接 (link)元素。                           |
| List Module (列表模块)                        | 定义列表元素ol, li, ul, dd, dt,和dl。           |
| Metainformation Module (元信息模块)           | 定义meta元素。                                  |
| Name Identification Module (名称识别模块)     | 定义已被废弃的name属性。                        |
| Object Module (对象模块)                      | 定义对象元素 (object)和param元素。              |
| Presentation Module (表现模块)                | 定义表现元素比如b和i。                          |
| Scripting Module (脚本模块)                   | 定义脚本 (script)和无脚本 (noscript)元素。      |
| Server Image Map Module(服务器端图像映射模块) | 定义服务器端图像映射(server side image map)元素 |
| Structure Module (结构模块)                   | 定义以下元素：html, head, title and body。      |
| Style Attribute Module (样式属性模块)         | 定义样式属性。                                  |
| Style Sheet Module (样式表模块)               | 定义样式元素。                                  |
| Tables Module (表格模块)                      | 定义用于表格中的元素。                          |
| Target Module (Target模块)                    | 定义target属性。                                |
| Text Module (文本模块)                        | 定义文本容器元素 (text container)，比如p和h1。  |



****

###***核心属性***

以下标签不提供下面的属性：base, head, html, meta, param, script, style, 以及 title 元素。

| 属性  | 值                       | 描述                   |
| :---- | :----------------------- | :--------------------- |
| class | class_rule 或 style_rule | 元素的类(class)        |
| id    | id_name                  | 元素的某个特定id       |
| style | 样式定义                 | 内联样式定义           |
| title | 提示文本                 | 显示于提示工具中的文本 |



### ***语言属性 (Language Attributes)***

以下标签不提供下面的属性：base, br, frame, frameset, hr, iframe, param, 以及 script 元素。

| 属性 | 值         | 描述                         |
| :--- | :--------- | :--------------------------- |
| dir  | ltr \| rtl | 设置文本的方向（从左或从右） |
| lang | 语言代码   | 设置语言代码                 |



### 事件触发

url：https://www.w3school.com.cn/xhtml/xhtml_eventattributes.asp

​		 https://www.w3school.com.cn/tags/html_ref_eventattributes.asp





## HTML5

```html5
<!DOCTYPE HTML>
```



视频支持

### video 标签的属性

| 属性                                                         | 值       | 描述                                                         |
| :----------------------------------------------------------- | :------- | :----------------------------------------------------------- |
| [autoplay](https://www.w3school.com.cn/tags/att_video_autoplay.asp) | autoplay | 如果出现该属性，则视频在就绪后马上播放。                     |
| [controls](https://www.w3school.com.cn/tags/att_video_controls.asp) | controls | 如果出现该属性，则向用户显示控件，比如播放按钮。             |
| [height](https://www.w3school.com.cn/tags/att_video_height.asp) | *pixels* | 设置视频播放器的高度。                                       |
| [loop](https://www.w3school.com.cn/tags/att_video_loop.asp)  | loop     | 如果出现该属性，则当媒介文件完成播放后再次开始播放。         |
| [preload](https://www.w3school.com.cn/tags/att_video_preload.asp) | preload  | 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 |
| [src](https://www.w3school.com.cn/tags/att_video_src.asp)    | *url*    | 要播放的视频的 URL。                                         |
| [width](https://www.w3school.com.cn/tags/att_video_width.asp) | *pixels* | 设置视频播放器的宽度。                                       |

url ：https://www.w3school.com.cn/html5/html_5_video.asp



### **拖放（Drag 和 drop）是 HTML5 标准的组成部分。**

https://www.w3school.com.cn/html5/html_5_draganddrop.asp

```html
<!DOCTYPE HTML>
<html>
<head>
<style type="text/css">
#div1, #div2
{float:left; width:198px; height:66px; margin:10px;padding:10px;border:1px solid #aaaaaa;}
</style>
<script type="text/javascript">
function allowDrop(ev)
{
ev.preventDefault();
}

function drag(ev)
{
ev.dataTransfer.setData("Text",ev.target.id);
}

function drop(ev)
{
ev.preventDefault();
var data=ev.dataTransfer.getData("Text");
ev.target.appendChild(document.getElementById(data));
}
</script>
</head>
<body>

<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)">
  <img src="/i/eg_dragdrop_w3school.gif" draggable="true" ondragstart="drag(event)" id="drag1" />
</div>
<div id="div2" ondrop="drop(event)" ondragover="allowDrop(event)"></div>

</body>
</html>

```



### **canvas 元素用于在网页上绘制图形**

https://www.w3school.com.cn/html5/html_5_canvas.asp

```html
<!DOCTYPE HTML>
<html>
<body>

<canvas id="myCanvas" width="200" height="100" style="border:1px solid #c3c3c3;">
Your browser does not support the canvas element.
</canvas>

<script type="text/javascript">

var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
cxt.fillStyle="#FF0000";
cxt.fillRect(0,0,200,100);

</script>

</body>
</html>
```



### **HTML5 支持内联 SVG。**

https://www.w3school.com.cn/html5/html_5_svg.asp



### 在客户端存储数据

HTML5 提供了两种在客户端存储数据的新方法：

- localStorage - 没有时间限制的数据存储
- sessionStorage - 针对一个 session 的数据存储



### **使用 HTML5，通过创建 cache manifest 文件，可以轻松地创建 web 应用的离线版本。**

https://www.w3school.com.cn/html5/html_5_app_cache.asp

Cache Manifest 基础

如需启用应用程序缓存，请在文档的 <html> 标签中包含 manifest 属性：

```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">
...
</html>
```



Manifest 文件

manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。

manifest 文件可分为三个部分：

- *CACHE MANIFEST* - 在此标题下列出的文件将在首次下载后进行缓存
- *NETWORK* - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- *FALLBACK* - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）





### **web worker 是运行在后台的 JavaScript，不会影响页面的性能。**

https://www.w3school.com.cn/html5/html_5_webworkers.asp

由于 web worker 位于外部文件中，它们无法访问下例 JavaScript 对象：

- window 对象
- document 对象
- parent 对象



### **HTML5 服务器发送事件（server-sent event）允许网页获得来自服务器的更新。**

**接收 Server-Sent 事件通知**

​	EventSource 对象用于接收服务器发送事件通知：

**EventSource 对象**

在上面的例子中，我们使用 onmessage 事件来获取消息。不过还可以使用其他事件：

| 事件      | 描述                     |
| :-------- | :----------------------- |
| onopen    | 当通往服务器的连接被打开 |
| onmessage | 当接收到消息             |
| onerror   | 当错误发生               |



###H5表单的输入和输出

https://www.w3school.com.cn/html5/html_5_form_input_types.asp

https://www.w3school.com.cn/html5/html_5_form_elements.asp



### HTML5 的新的表单属性

本章讲解涉及 <form> 和 <input> 元素的新属性。

https://www.w3school.com.cn/html5/html_5_form_attributes.asp

正则匹配，默认输入等





## CSS

###***css语法***

```css
selector {property: value}
```



```css
p { color: rgb(255,0,0); }
p { color: rgb(100%,0%,0%); }
```

注意，当使用 RGB 百分比时，即使当值为 0 时也要写百分比符号。但是在其他的情况下就不需要这么做了。比如说，当尺寸为 0 像素时，0 之后不需要使用 px 单位，因为 0 就是 0，无论单位是什么。



**选择器的分组**

```css
h1,h2,h3,h4,h5,h6 {
  color: green;
  }
```

根据 CSS，子元素从父元素继承属性



创建一个针对 p 的特殊规则，这样它就会***摆脱父元素的规则***：

```css
body  {
     font-family: Verdana, sans-serif;
     }

td, ul, ol, ul, li, dl, dt, dd  {
     font-family: Verdana, sans-serif;
     }

p  {
     font-family: Times, "Times New Roman", serif;
     }
```



### **css派生选择器**

https://www.w3school.com.cn/css/css_syntax_descendant_selector.asp



### **CSS id 选择器**

Id选择器+派生选择器

```css
#sidebar p {
	font-style: italic;
	text-align: right;
	margin-top: 0.5em;
	}
```



### **在 CSS 中，类选择器以一个点号显示：**

```css
.center {text-align: center}
```

在下面的 HTML 代码中，h1 和 p 元素都有 center 类。这意味着两者都将遵守 ".center" 选择器中的规则。

```html
<h1 class="center">
This heading will be center-aligned
</h1>

<p class="center">
This paragraph will also be center-aligned.
</p>
```

**注意：**类名的第一个字符不能使用数字！它无法在 Mozilla 或 Firefox 中起作用。



```css
.fancy td {
	color: #f60;
	background: #666;
	}
```

```css
td.fancy {
	color: #f60;
	background: #666;
	}
```



### CSS 属性选择器

https://www.w3school.com.cn/css/css_syntax_attribute_selector.asp

```css
[title]
{
color:red;
}
```

所有的title属性的都会适应



 <!--适用于由空格分隔的属性值：-->

```css
[title~=hello] { color:red; }
```

<!--适用于由连字符分隔的属性值;-->

```css
[lang|=en] { color:red; }
```



### 如何创建 CSS

####外部🔗

```css 
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```



####内部样式表

```css
<head>
<style type="text/css">
  hr {color: sienna;}
  p {margin-left: 20px;}
  body {background-image: url("images/back40.gif");}
</style>
</head>
```



####内联样式

由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。

要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：

```
<p style="color: sienna; margin-left: 20px">
This is a paragraph
</p>
```



优先级逻辑：内联 > 内部样式表 > 外部



### CSS 背景

#### 定位信息

https://www.w3school.com.cn/css/css_background.asp

```css
background-position
```



###CSS 字体

https://www.w3school.com.cn/css/css_font.asp

建议在所有``` font-family ```规则中都提供一个通用字体系列。这样就提供了一条后路，在用户代理无法提供与规则匹配的特定字体时，就可以选择一个候选字体。

```css
p {font-family: Times, TimesNR, 'New Century Schoolbook',
     Georgia, 'New York', serif;}
```

```html
<p style="font-family: Times, TimesNR, 'New Century Schoolbook', Georgia,
 'New York', serif;">...</p>
```



字体大小

```css
h1 {font-size:3.75em;} /* 60px/16=3.75em */
h2 {font-size:2.5em;}  /* 40px/16=2.5em */
p {font-size:0.875em;} /* 14px/16=0.875em */
```



### CSS链接

```html
<!DOCTYPE html>
<html>
<head>
<style>
a:link {background-color:#B2FF99;}    /* 未被访问的链接 */
a:visited {background-color:#FFFF85;} /* 已被访问的链接 */
a:hover {background-color:#FF704D;}   /* 鼠标指针移动到链接上 */
a:active {background-color:#FF704D;}  /* 正在被点击的链接 */
</style>
</head>

<body>
<p><b><a href="/index.html" target="_blank">W3School</a></b></p>
<p><b><a href="http://wwf.panda.org/" target="_blank">WWF</a></b></p>

<p><b>注释：</b>为了使定义生效，a:hover 必须位于 a:link 和 a:visited 之后！！</p>
<p><b>注释：</b>为了使定义生效，a:active 必须位于 a:hover 之后！！</p>
</body>
</html>
```

 

### CSS 列表

https://www.w3school.com.cn/css/css_list.asp

list-style



### CSS框模型

![CSS 框模型](https://www.w3school.com.cn/i/ct_boxmodel.gif)

#### 内边距

以16:9的比例，然后上下浮动进行

```css
h1 {padding: 10px;}
```



#### 外框

***border***属性

1、 定义多种样式

您可以为一个边框定义多个样式，例如：

```css
p.aside {border-style: solid dotted dashed double;}
```

上面这条规则为类名为 aside 的段落定义了四种边框样式：实线上边框、点线右边框、虚线下边框和一个双线左边框。

我们又看到了这里的值采用了 top-right-bottom-left 的顺序，讨论用多个值设置不同内边距时也见过这个顺序。

#### CSS 外边距

https://www.w3school.com.cn/css/css_margin.asp

***margin*** 属性

```html
<html>
<head>
<style type="text/css">
p.margin {margin: 2cm 4cm 3cm 4cm;
		  border-style: solid}
</style>
</head>

<body>

<p>这个段落没有指定外边距。</p>

<p class="margin">这个段落带有指定的外边距。这个段落带有指定的外边距。这个段落带有指定的外边距。这个段落带有指定的外边距。这个段落带有指定的外边距。</p>

<p>这个段落没有指定外边距。</p>

</body>

</html>
```



####*CSS外边距合并

https://www.w3school.com.cn/css/css_margin_collapsing.asp

**注释：**只有普通文档流中块框的垂直外边距才会发生外边距合并。



###CSS 定位机制

CSS 有三种基本的定位机制：普通流、浮动和绝对定位。

[z-index](https://www.w3school.com.cn/cssref/pr_pos_z-index.asp)

设置图片的优先级



#### css绝对位置

https://www.w3school.com.cn/css/css_positioning_absolute.asp

绝对定位的元素的位置相对于*最近的已定位祖先元素*，如果元素没有已定位的祖先元素，那么它的位置相对于*最初的包含块*。

```html
<html>
<head>
<style type="text/css">
div {
position:absolute;
left:100px;
top:150px}
h2.pos_abs
{
position:absolute;
left:100px;
top:150px
}
</style>
</head>

<body>
<div>
<h2 class="pos_abs">这是带有绝对定位的标题</h2>
<p>通过绝对定位，元素可以放置到页面上的任何位置。下面的标题距离页面左侧 100px，距离页面顶部 150px。</p>
</div>
</body>

</html>
```



####CSS浮动

展现出 ***并列*** 的功效 水平菜单

```html
<html>
<head>
<style type="text/css">
ul
{
float:left;
width:100%;
padding:0;
margin:0;
list-style-type:none;
}
a
{
float:left;
width:7em;
text-decoration:none;
color:white;
background-color:purple;
padding:0.2em 0.6em;
border-right:1px solid white;
}
a:hover {background-color:#ff3300}
li {display:inline}
</style>
</head>

<body>
<ul>
<li><a href="#">Link one</a></li>
<li><a href="#">Link two</a></li>
<li><a href="#">Link three</a></li>
<li><a href="#">Link four</a></li>
</ul>

<p>
在上面的例子中，我们把 ul 元素和 a 元素浮向左浮动。li 元素显示为行内元素（元素前后没有换行）。这样就可以使列表排列成一行。ul 元素的宽度是 100%，列表中的每个超链接的宽度是 7em（当前字体尺寸的 7 倍）。我们添加了颜色和边框，以使其更漂亮。
</p>

</body>
</html>
```



[***clear 属性***](https://www.w3school.com.cn/cssref/pr_class_clear.asp)

```html
<html>

<head>
<style type="text/css">
img
  {
  float:left;
  clear:left; <!-- 等价于both -->
  }
</style>
</head>

<body>
<img src="/i/eg_smile.gif" />
<img src="/i/eg_smile.gif" />
</body>

</html>
```



### CSS选择器

xm文档也可以使用

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<?xml-stylesheet type="text/css" href="note.css"?>
```



#### 分组

* group 标签和元素

* 通配符选择器

  ```css
  * {color:red;}
  ```

* 声明进行分组



#### CSS 类选择器

https://www.w3school.com.cn/css/css_selector_class.asp

#####*CSS 多类选择器

```html
<p class="important warning">
This paragraph is a very important warning.
</p>
```

```css
.important {font-weight:bold;}
.warning {font-style:italic;}
.important.warning {background:silver;}
```

有点意思

```css
.important.urgent {background:silver;}
```

ps：必须包含important和urgent两个类名



#### 属性选择器

根据 ***标签*** 和 ***属性*** 对照对应的样式

ps：属性与属性值必须完全匹配

```html
<p class="important warning">This paragraph is a very important warning.</p>
```

```css
p[class="important warning"] {color: red;}
```



ps:如果需要根据属性值中的词列表的某个词进行选择，则需要使用波浪号（~）。

```css
p[class~="important"] {color: red;}

```



#####*子串匹配属性选择器

下面为您介绍一个更高级的选择器模块，它是 CSS2 完成之后发布的，其中包含了更多的部分值属性选择器。按照规范的说法，应该称之为“子串匹配属性选择器”。

| 类型         | 描述                                       |
| :----------- | :----------------------------------------- |
| [abc^="def"] | 选择 abc 属性值以 "def" 开头的所有元素     |
| [abc$="def"] | 选择 abc 属性值以 "def" 结尾的所有元素     |
| [abc*="def"] | 选择 abc 属性值中包含子串 "def" 的所有元素 |



####后代选择器

```css
h1 em {color:red;}
```



#### 子元素选择器

```css
h1 > strong {color:red;}
```



下面的例子结合上面 ***后代*** 和 ***子元素*** 非常合适

```html
<h1>This is <strong>very</strong> <strong>very</strong> important.</h1>
<h1>This is <em>really <strong>very</strong></em> important.</h1>
```



#### 相邻兄弟元素选择器

```html
<!DOCTYPE HTML>
<html>
<head>
<style type="text/css">
h1 + p {border: 1px solid gray}
</style>
</head>

<body>
<h1>This is a heading.</h1>
<p>This is paragraph.</p>
<p>This is paragraph.</p>
<p>This is paragraph.</p>
<p>This is paragraph.</p>
<p>This is paragraph.</p>
</body>
</html>

```



#### 伪类

作用于标签上，体现了 ***元素*** 的功能

https://www.w3school.com.cn/css/css_pseudo_classes.asp

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<html>
<head>
<style type="text/css">
p > i :first-child{
  font-weight:bold;
  } 
</style>
</head>

<body>
<p>some <q><i>text</i></q>. some <i>text</i>.</p>
<p>some <i>text</i>. some <i>text</i>.</p>
</body>
</html>
```

| 属性                                                         | 描述                                     | CSS  |
| :----------------------------------------------------------- | :--------------------------------------- | :--- |
| [:active](https://www.w3school.com.cn/cssref/pr_pseudo_active.asp) | 向被激活的元素添加样式。                 | 1    |
| [:focus](https://www.w3school.com.cn/cssref/pr_pseudo_focus.asp) | 向拥有键盘输入焦点的元素添加样式。       | 2    |
| [:hover](https://www.w3school.com.cn/cssref/pr_pseudo_hover.asp) | 当鼠标悬浮在元素上方时，向元素添加样式。 | 1    |
| [:link](https://www.w3school.com.cn/cssref/pr_pseudo_link.asp) | 向未被访问的链接添加样式。               | 1    |
| [:visited](https://www.w3school.com.cn/cssref/pr_pseudo_visited.asp) | 向已被访问的链接添加样式。               | 1    |
| [:first-child](https://www.w3school.com.cn/cssref/pr_pseudo_first-child.asp) | 向元素的第一个子元素添加样式。           | 2    |
| [:lang](https://www.w3school.com.cn/cssref/pr_pseudo_lang.asp) | 向带有指定 lang 属性的元素添加样式。     | 2    |



#### 伪元素

作用于内容上，体现了 ***元素*** 的功能

https://www.w3school.com.cn/css/css_pseudo_elements.asp

| 属性                                                         | 描述                             | CSS  |
| :----------------------------------------------------------- | :------------------------------- | :--- |
| [:first-letter](https://www.w3school.com.cn/cssref/pr_pseudo_first-letter.asp) | 向文本的第一个字母添加特殊样式。 | 1    |
| [:first-line](https://www.w3school.com.cn/cssref/pr_pseudo_first-line.asp) | 向文本的首行添加特殊样式。       | 1    |
| [:before](https://www.w3school.com.cn/cssref/pr_pseudo_before.asp) | 在元素之前添加内容。             | 2    |
| [:after](https://www.w3school.com.cn/cssref/pr_pseudo_after.asp) | 在元素之后添加内容。             | 2    |





###*CSS 选择器参考手册

| 选择器                                                       | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [[*attribute*\]](https://www.w3school.com.cn/cssref/selector_attribute.asp) | 用于选取带有指定属性的元素。                                 |
| [[*attribute*=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value.asp) | 用于选取带有指定属性和值的元素。                             |
| [[*attribute*~=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | 用于选取属性值中包含指定词汇的元素。                         |
| [[*attribute*\|=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | 用于选取带有以指定值开头的属性值的元素，该值必须是整个单词。 |
| [[*attribute*^=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_begin.asp) | 匹配属性值以指定值开头的每个元素。                           |
| [[*attribute*$=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_end.asp) | 匹配属性值以指定值结尾的每个元素。                           |
| [[*attribute**=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_contain.asp) | 匹配属性值中包含指定值的每个元素。                           |





###*类选择器还是 ID 选择器？

在类选择器这一章中我们曾讲解过，可以为任意多个元素指定类。前一章中类名 important 被应用到 p 和 h1 元素，而且它还可以应用到更多元素。

####区别 1：只能在文档中使用一次

与类不同，在一个 HTML 文档中，ID 选择器会使用一次，而且仅一次。

####区别 2：不能使用 ID 词列表

不同于类选择器，ID 选择器不能结合使用，因为 ID 属性不允许有以空格分隔的词列表。

####区别 3：ID 能包含更多含义

类似于类，可以独立于元素来选择 ID。有些情况下，您知道文档中会出现某个特定 ID 值，但是并不知道它会出现在哪个元素上，所以您想声明独立的 ID 选择器。例如，您可能知道在一个给定的文档中会有一个 ID 值为 mostImportant 的元素。您不知道这个最重要的东西是一个段落、一个短语、一个列表项还是一个小节标题。您只知道每个文档都会有这么一个最重要的内容，它可能在任何元素中，而且只能出现一个。在这种情况下，可以编写如下规则：

####*区分大小写

请注意，类选择器和 ID 选择器可能是区分大小写的。这取决于文档的语言。HTML 和 XHTML 将类和 ID 值定义为区分大小写，所以类和 ID 值的大小写必须与文档中的相应值匹配。





### CSS高级

#### ex水平导航栏

```html
<!DOCTYPE html>
<html>
<head>
<style>
ul
{
list-style-type:none;
margin:0;
padding:0;
overflow:hidden;
}
li
{
float:left;
}
a:link,a:visited
{
display:block;
width:120px;
font-weight:bold;
color:#FFFFFF;
background-color:#bebebe;
text-align:center;
padding:4px;
text-decoration:none;
text-transform:uppercase;
}
a:hover,a:active
{
background-color:#cc0000;
}

</style>
</head>

<body>
<ul>
<li><a href="#home">Home</a></li>
<li><a href="#news">News</a></li>
<li><a href="#contact">Contact</a></li>
<li><a href="#about">About</a></li>
</ul>
</body>
</html>
```



通过 CSS 创建透明图像是很容易的。

**注释：**CSS ***opacity*** 属性是 W3C CSS 推荐标准的一部分。













# JS记录本



## 重新开门

<p id="demo">JavaScript 能够改变 HTML 内容。</p>

```html
<button type="button" onclick='document.getElementById("demo").innerHTML = "Hello JavaScript!"'>点击我！</button>
```

**提示：**JavaScript 同时接受双引号和单引号：



<p>在本例中，JavaScript 改变了图像的 src 属性值。</p>

```html
<button onclick="document.getElementById('myImage').src='/i/eg_bulbon.gif'">开灯</button>

<img id="myImage" border="0" src="/i/eg_bulboff.gif" style="text-align:center;">

<button onclick="document.getElementById('myImage').src='/i/eg_bulboff.gif'">关灯</button>
```



<p id="demo">JavaScript 能够改变 HTML 元素的样式。</p>

```html
<button type="button" onclick="document.getElementById('demo').style.fontSize='35px'">
点击我！
</button>
```



<p id="demo" >JavaScript 能够改变 HTML 元素的样式。</p>

```html
<button type="button" onclick="document.getElementById('demo').style.fontSize='35px'">
点击我！
</button>
```



