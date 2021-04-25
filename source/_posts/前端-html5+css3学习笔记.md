---
title: HTML+CSS笔记
date: 2021-01-30 21:38:48
tags: 前端

---

前端知识整理，以备不时之需。

<!--more-->

写在开始：本文部分内容直接搬运自一些网站，仅用作个人和他人学习用途。

## 资源整理

### HTML

[菜鸟教程](https://www.runoob.com/html/html-tutorial.html)   [w3school](https://www.w3school.com.cn/html/index.asp)  

### CSS

[菜鸟教程](https://www.runoob.com/css/css-tutorial.html)  [w3school](https://www.w3school.com.cn/css/index.asp)  

## 慕课网教程笔记

[慕课网入门教程](https://www.imooc.com/learn/9)

### 1.HTML文档结构

```html
<!DOCTYPE html> 文档类型声明，表示该文件为 HTML5文件。<!DOCTYPE> 声明必须是 HTML 文档的第一行，位于 <html> 标签之前
<html>
    <head>  -->head标签表示头部标签,通常用来嵌套meta、title、style等标签。 title:网页标题
        <meta charset="UTF-8">
        <title>认识html文件基本结构</title>
    </head>
    <body>
        <h1>在本小节中，你将学会认识html文件基本结构</h1>
    </body>
</html>
```

### 2.语义化标签

```html
<p>段落文本</p>
<span>自定义文本标签</span>
<h1>标题标签 h1-h6来表示</h1>
<div>块标签</div>
<header>网页开头</header>
<footer>网页底部</footer>
<section>区域</section>
<aside>侧边栏</aside>
```

### 3.效果标签

```html
<br/> -->换行标签
&nbsp; -->空格
<hr/> -->水平分割线
```

### 4.列表标签

```html
<ul>
	<li> </li> -->无序列表
</ul> 
<ol>
    <li>abc</li> -->有序列表
    <li>def</li>
</ol>
```

### 5.图片链接和表格标签

```html
<img src="图片地址" alt="下载失败时的替换文本" title = "鼠标滑过时的提示文本">
<a  href="目标网址"  title="鼠标滑过显示的文本" target="_self(覆盖原网页)/_blank(打开新网页)">链接显示的文本</a>
<table border=10 > -->table表格标签 border属性为边框粗细
    <tr> -->tr表示行
        <th>知识点</th> -->th表示第一行加粗的表格表头单元格
    </tr>
    <tr>
        <td>html</td> -->td表示普通单元格
    </tr>
</table>
<thead>标签定义表格头部,<tbody>标签来定义表格的内容,<tfoot>来定义表格的底部,当长的表格被打印时，表格的表头和页脚可被打印在包含表格数据的每张页面上。
```

### 6.表单标签
```html
<form method="post" action="save.php"> -->action:浏览者输入的数据被传送到的地方,比如一个PHP页面(save.php) 
    								   -->method:数据传送的方式（get/post）
    <!--文本输入-->
    <label for="username">用户名:</label>
    <input type="text" name="username" id="username" value="" placeholder=""/> -->普通文本输入
    <label for="pass">密码:</label>
    <input type="password" name="pass" id="pass" value="" placeholder=""/> -->加密文本输入
    -->for=id完成了和input标签的联动，和相应的输入框进行绑定
    -->type="num"--只允许输入数字 type="url"--只允许输入http/https协议的网址 type="email"-->邮箱
    <textarea  rows="行数" cols="列数">文本</textarea> -->大段文字输入框
	
    <!--选择框-->
	<input type="radio/checkbox" value="值" name="名称" checked="checked"/>
     -->当 type="radio" 时，控件为单选框
   	    当 type="checkbox" 时，控件为复选框
	
    <!--下拉菜单-->
    <select> -->下拉菜单 select标签里面只能放option标签，表示下拉列表的选项。option标签放选项内容，不放置其他标签
        <option value="看书">看书</option>
        <option value="旅游">旅游</option>
        <option value="运动" selected="selected">运动</option>
        <option value="购物">购物</option>
    </select>
    
    <!--按钮--> -->对表单其他元素做相应的操作
    <input type="submit" value="确定" name="submit" />  -->value：按钮上显示的文字
    <input type="reset" value="重置" name="reset" />  
</form>


```

#### 几个常用属性

value：提交数据到服务器的值（后台程序PHP使用）

name：为控件命名，以备后台程序 ASP、PHP 使用

checked：当设置 checked="checked" 时，该选项被默认选中

### 7.CSS样式基本使用

css 样式由**选择符**和**声明**组成，而**声明**又由**属性**和**值**组成

注释：`/* */`

#### 内联样式

```css
<p style="color:red;font-size:12px">这里文字是红色。</p>
```

#### 嵌入式

```css
<style type="text/css">
    span {
        color:blue;
    }
</style>

<span>123</span>
```

#### 外联样式

一般写在`<head>`标签内

```css
<link href="base.css" rel="stylesheet" type="text/css" /> 
```

#### 三种方式优先级

内联式 > 嵌入式 > 外部式

即**就近原则**

### 8.CSS选择器

声明组成：

```css
选择器 {
    样式
}
```

#### 各种选择器

```css
/* 标签选择器 */
p {font-size:12px;line-height:1.6em;}

/* 类选择器 .类名 */
.类选器名称 {css样式代码;}

/* ID选择器 #ID */
#ID名字 {css样式;}

/*子选择器 > */
.food>li {border:1px solid red;}

/*后代选择器 空格 */
.food span{color:red;}

/*通用选择器 匹配html中所有标签元素*/
* {color:red;}

/* 伪类选择器 */
a:hover{color:red;} /*给html中一个标签元素的鼠标滑过的状态来设置字体颜色*/

/* 分组选择器 */
h1,span{
    color:red;
}
```

#### 类和ID选择器区别

* **ID选择器只能在文档中使用一次**。与类选择器不同，在一个HTML文档中，ID选择器只能使用一次，而且仅一次。而类选择器可以使用多次。换言之，每个标签ID不能相同

* **可以使用类选择器词列表方法为一个元素同时设置多个样式。**我们可以为一个元素同时设多个样式，但只可以用类选择器的方法实现，ID选择器是不可以的（**不能使用 ID 词列表**）。如：

```css
.stress{
    color:red;
}
.bigsize{
    font-size:25px;
}
<p>到了<span class="stress bigsize">三年级</span>下学期时，我们班上了一节公开课...</p>
```

### 9.CSS继承，优先级和重要性

#### 优先级

内联样式 > id选择器 > 类选择器 > 标签选择器 > 通配符选择器

#### 权值计算-特殊性

标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100。继承的权值可视为0.1

#### 选择器最高层级!important

```css
p{color:red!important;}
```

### 10.CSS字体样式

```css
/*使用font-family设置字体*/
body{font-family:"宋体";}

/*使用font-size设置字体大小*/
body{font-size:12px;}

/*使用font-weight设置字体粗细*/
p span{font-weight:bold;}

/*使用font-style设置字体样式
正常字体为normal,也是font-style的默认值。
italic为设置字体为斜体，用于字体本身就有倾斜的样式。
oblique为设置倾斜的字体，强制将字体倾斜。*/

/*使用color设置字体颜色 三种赋值方式*/
p{color:red;}
p{color:rgb(133,45,200);}
p{color:#00ffff;}

/*font样式的简写方式*/
body{
    font-style:italic;
    font-weight:bold; 
    font-size:12px; 
    line-height:1.5em; 
    font-family:"宋体",sans-serif;
}
--------------->
body{
    font:italic  bold  12px/1.5em  "宋体",sans-serif;
}
```

### 11.装饰文本

```css
/* text-decoration添加文本修饰
1、text-decoration可以设置添加到文本的修饰。
2、text-decoration默认值为none, 定义标准的文本。
3、text-decoration的值为underline为定义文本下的一条线。
4、text-decoration的值为overline为定义文本上的一条线。
5、text-decoration的值为line-through为定义穿过文本下的一条线，一般用于商品折扣价。 */

/*text-indent为文本添加首行缩进*/
p{text-indent:2em;}

/*line-height设置行间距*/
p{line-height:1.5em;}

/*使用letter/word-spacing增加或减少字符间的空白*/
h1{
    letter-spacing:50px;
}

/*使用text-align设置文本对齐方式*/
h1{
    text-align:center; /*可选left right*/
}
```

常用到`px（像素）`、`em`、`% 百分比` 三种长度单位，这三个都是相对长度单位

1em和font-size对应

百分比也是相对于font-size设定值的比

### 12.CSS盒模型

#### 元素分类

##### 常用的块状元素

```css
<div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>
```

##### 常用的内联元素

```css
<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>
```

##### 常用的内联块状元素

```css
<img>、<input>
```

##### 几种元素转换

```css
1.将a标签转为块级标签
a{display:block;}
2.将块状元素div转换为内联元素
div{
     display:inline;
}
3.设置为内联块状
display:inline-block;
4.none设置不显示
display:none;
```

#### 盒子模型

辅助理解的视频：https://www.imooc.com/video/3225

内容被`padding`，`border`，`margin`三层包住，每层都有一定的宽度。

都可以设置`top`/`bottom`/`left`/`right`四个方向的属性

##### 基本设置盒子模型

```css
div{
    width:200px;
    padding:20px;
    border:1px solid red;
    margin:10px;    
    background-color:red; /*设置背景色*/
}
```

<img src="https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/CSS3%2BHTML5/1.jpg" style="zoom:80%;" /> <img src="https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/CSS3%2BHTML5/2.jpg" style="zoom:80%;" />

#### 设置盒子边框

盒子模型的边框就是围绕着内容及补白的线，这条线你可以设置它的粗细、样式和颜色(边框三个属性)。

```css
div{
    border:2px  solid  red;
}
上面是 border 代码的缩写形式，可以分开写：
div{
    border-width:2px; /*不常用的:thin | medium | thick*/
    border-style:solid; /*边框样式: dashed（虚线）| dotted（点线）| solid（实线）。*/
    border-color:red;
}
/*只加底部边框*/
div{
    border-bottom:2px solid red; 
}

/*元素边框的圆角效果可以使用border-radius属性来设置。圆角可分为左上、右上、右下、左下。*/
div{border-radius: 20px 10px 15px 30px;}
```

#### 设置内边距

元素内容与边框之间是可以设置距离的，称之为“内边距（填充）”。填充也可分为上、右、下、左(顺时针)。

```css
div{padding:20px 10px 15px 30px;}

如果上、右、下、左的填充都为10px;可以这么写
div{padding:10px;}

如果上下填充一样为10px，左右一样为20px，可以这么写：
div{padding:10px 20px;}
```

#### 设置外边距

元素与其它元素之间的距离可以使用边界（margin）来设置。边界也是可分为上、右、下、左。使用方法和内边距一样。

```css
div{margin:20px 10px 15px 30px;}
```

### 13.CSS3布局模型

在网页中，元素有三种布局模型：
1、流动模型（Flow）
2、浮动模型 (Float)
3、层模型（Layer）

#### 流动模型

##### 典型特征

第一点，**块状元素**都会在所处的**包含元素内**自上而下按顺序垂直延伸分布，因为在默认状态下，块状元素的宽度都为**100%**。实际上，块状元素都会以行的形式占据位置。如右侧代码编辑器中三个块状元素标签(div，h1，p)宽度显示为100%。

第二点，在流动模型下，**内联元素**都会在所处的包含元素内从左到右水平分布显示。（内联元素可不像块状元素这么霸道独占一行）

#### 浮动模型

```css
/*让两个块状元素并排显示*/
div{
    width:200px;
    height:200px;
    border:2px red solid;
    float:left; /*左对齐 right即为右对齐*/
}
<div id="div1"></div>
<div id="div2"></div>

/*设置div1和div2一个最左一个最右*/
div{
    width:200px;
    height:200px;
    border:2px red solid;
}
#div1{float:left;}
#div2{float:right;} 

<div id="div1"></div>
<div id="div2"></div>
```

#### 层模型

层模型有三种形式：

1、绝对定位(position: absolute)

2、相对定位(position: relative)

3、固定定位(position: fixed)

##### 绝对定位

如果想为元素设置层模型中的绝对定位，需要设置position:absolute(表示绝对定位)，这条语句的作用将元素**从文档流中拖出来**，然后使用left、right、top、bottom属性**相对于**其**最接近的一个具有定位属性**的**父包含块**进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于**浏览器窗口**。

```css
实现div元素相对于浏览器窗口向右移动100px，向下移动50px。
div{
    width:200px;
    height:200px;
    border:2px red solid;
    position:absolute;
    left:100px;
    top:50px;                                          
}
<div id="div1"></div>
```

##### 相对定位

如果想为元素设置层模型中的相对定位，需要设置position:relative（表示相对定位），它通过left、right、top、bottom属性确定元素在**正常文档流中**的偏移位置。相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于**以前的位置移动，**移动的方向和幅度由left、right、top、bottom属性确定，偏移前的位置保留不动。

```css
相对于以前位置向下移动50px，向右移动100px;
#div1{
    width:200px;
    height:200px;
    border:2px red solid;
    position:relative;
    left:100px;
    top:50px;
}
<div id="div1"></div>
```

##### 固定定位

fixed：表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是视图（屏幕内的网页窗口）本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终位于浏览器窗口内视图的某个位置，不会受文档流动影响，这与background-attachment:fixed;属性功能相同。

```css
实现相对于浏览器视图向右移动100px，向下移动50px。并且拖动滚动条时位置固定不变。
#div1{
    width:200px;
    height:200px;
    border:2px red solid;
    position:fixed;
    left:100px;
    top:50px;
}
<p>文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本。</p>
```

##### Relative与Absolute组合使用

使用相对于某个元素的绝对定位

```css
1、参照定位的元素必须是相对定位元素的前辈元素：
<div id="box1"><!--参照定位的元素-->
    <div id="box2">相对参照元素进行定位</div><!--相对定位元素-->
</div>
从上面代码可以看出box1是box2的父元素（父元素当然也是前辈元素了）。

2、参照定位的元素必须加入position:relative;
#box1{
    width:200px;
    height:200px;
    position:relative;        
}

3、定位元素加入position:absolute，便可以使用top、bottom、left、right来进行偏移定位了。
#box2{
    position:absolute;
    top:20px;
    left:30px;         
}
这样box2就可以相对于父元素box1定位了（这里注意参照物就可以不是浏览器了，而可以自由设置了）。
```

### 14.弹性盒模型

#### 设置flex

三个块元素设置大小以及背景色，在父容器中添加flex。

技术点的解释：

1、设置display: flex属性可以把块级元素在一排显示。

2、flex需要添加在父元素上，改变子元素的排列顺序。

3、默认为从左往右依次排列,且和父元素左边没有间隙。

```css
<div class="box">
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
</div>
<style type="text/css">
    .box {
        background: blue;
        display: flex;
    }
    .box div {
        width: 200px;
        height: 200px;
    }
    .box1 {
        background: red;
    }
    .box2 {
        background: orange;
    }
    .box3 {
        background: green;
    }
</style>
```

#### 使用justify-content属性设置横轴排列方式

```css
 justify-content: flex-start | flex-end | center | space-between | space-around;
```

`flex-start`：交叉轴的起点对齐

```css
 .box {
        background: blue;
        display: flex;
        justify-content: flex-start;
    }
```

实现效果：

[![img](https://img.mukewang.com/5e959b080001a38d25340322.jpg)](https://img.mukewang.com/5e959b080001a38d25340322.jpg)

`flex-end`：右对齐

```css
 .box {
        background: blue;
        display: flex;
        justify-content: flex-end;
    }
```

实现效果：

[![img](https://img1.mukewang.com/5e959b8b0001d43b25420308.jpg)](https://img1.mukewang.com/5e959b8b0001d43b25420308.jpg)

`center`： 居中

```css
 .box {
        background: blue;
        display: flex;
        justify-content: center;
    }
```

实现效果：

[![img](https://img2.mukewang.com/5e959bdd0001ad2125300303.jpg)](https://img2.mukewang.com/5e959bdd0001ad2125300303.jpg)

`space-between`：两端对齐，项目之间的间隔都相等。

```css
 .box {
        background: blue;
        display: flex;
        justify-content: space-between;
    }
```

实现效果：

[![img](https://img2.mukewang.com/5e959c6400017b1c25530313.jpg)](https://img2.mukewang.com/5e959c6400017b1c25530313.jpg)

`space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

```css
.box {
        background: blue;
        display: flex;
        justify-content: space-around;
    }
```

实现效果：

[![img](https://img2.mukewang.com/5e959caf000113b125370303.jpg)](https://img2.mukewang.com/5e959caf000113b125370303.jpg)

#### 使用align-items属性设置纵轴排列方式

```css
align-items: flex-start | flex-end | center | baseline | stretch;
```

`flex-start`：默认值，左对齐

```css
   .box {
        height: 700px;
        background: blue;
        display: flex;
        align-items: flex-start;
    }
```

实现效果：

[![img](https://img3.mukewang.com/5e95a3720001140325381051.jpg)](https://img3.mukewang.com/5e95a3720001140325381051.jpg)

`flex-end`：交叉轴的终点对齐

```css
 .box {
        height: 700px;
        background: blue;
        display: flex;
        align-items: flex-end;
    }
```

实现效果：

[![img](https://img2.mukewang.com/5e95a3ca0001550a25381056.jpg)](https://img2.mukewang.com/5e95a3ca0001550a25381056.jpg)

`center`： 交叉轴的中点对齐

```css
.box {
        height: 700px;
        background: blue;
        display: flex;
        align-items: center;
    }
```

实现效果：

[![img](https://img3.mukewang.com/5e9667880001796c25371056.jpg)](https://img3.mukewang.com/5e9667880001796c25371056.jpg)

`baseline`：项目的第一行文字的基线对齐。

```css
.box {
        height: 700px;
        background: blue;
        display: flex;
        align-items: baseline;
    }
```

三个盒子中设置不同的字体大小，可以参考右侧编辑器中的代码进行测试。

实现效果：

[![img](https://img3.mukewang.com/5e9668ff0001f8f125341053.jpg)](https://img3.mukewang.com/5e9668ff0001f8f125341053.jpg)

`stretch（默认值）`：如果项目未设置高度或设为auto，将占满整个容器的高度。

```css
 .box {
        height: 300px;
        background: blue;
        display: flex;
        align-items: stretch;
    }

    .box div {
        /*不设置高度，元素在垂直方向上铺满父容器*/
        width: 200px;
    }
```

实现效果：

[![img](https://img2.mukewang.com/5e9669ef00017e0e25390453.jpg)](https://img2.mukewang.com/5e9669ef00017e0e25390453.jpg)

#### 给子元素设置flex占比

[![img](https://img3.mukewang.com/5e966c3100011c5b25430450.jpg)](https://img3.mukewang.com/5e966c3100011c5b25430450.jpg)

**技术点的解释：**

1、给子元素设置flex属性,可以设置子元素相对于父元素的占比。

2、flex属性的值只能是正整数,表示占比多少。其实是几个子块的比

3、给子元素设置了flex之后,其宽度属性会失效。

```css
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>flex占比</title>
    <style type="text/css">
    .box {
        height: 300px;
        background: blue;
        display: flex;
    }

    .box div {
        width: 200px;
        height: 200px;
    }

    .box1 {
        flex: 1;
        background: red;
    }

    .box2 {
        flex: 3;
        background: orange;
    }

    .box3 {
        flex: 2;
        background: green;
    }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">flex:1</div>
        <div class="box2">flex:3</div>
        <div class="box3">flex:2</div>
    </div>
</body>

</html>
```

### 15.css样式设置技巧

#### 水平居中设置-行内元素

如果被设置元素为文本、图片等行内元素时，水平居中是通过给父元素设置 `text-align:center` 来实现的。

#### 水平居中设置-定宽块状元素

当被设置元素为 块状元素 时用 `text-align：center` 就不起作用了，这时也分两种情况：**定宽块状元素**和**不定宽块状元素**。

##### 定宽块状元素：块状元素的宽度width为固定值。

满足定宽和块状两个条件的元素是可以通过设置“左右margin”值为“auto”来实现居中的。

```css
<body>
  <div>我是定宽块状元素，哈哈，我要水平居中显示。</div>
</body>

<style>
div{
    border:1px solid red;/*为了显示居中效果明显为 div 设置了边框*/
    width:200px;/*定宽*/
    margin:20px auto;/* margin-left 与 margin-right 设置为 auto */
}
</style>
```

#### 面试常考题之已知宽高实现盒子水平垂直居中

这一章节我们来学习已知宽高实现盒子水平垂直居中。通常使用定位完成，例如想要实现以下效果：

[![img](https://img4.mukewang.com/5e967bb40001e35725570475.jpg)](https://img4.mukewang.com/5e967bb40001e35725570475.jpg)

我们有如下两个div元素

![img](https://img3.mukewang.com/5e967a0500013c9104650162.jpg)

要实现子元素相对于父元素垂直水平居中,我们只需要输入以下代码：

[![img](https://img.mukewang.com/5e967a380001840f11050487.jpg)](https://img.mukewang.com/5e967a380001840f11050487.jpg)

**技术点的解释：**

1、利用父元素设置相对定位,子元素设置绝对定位,那么子元素就是相对于父元素定位的特性。

2、子元素设置上和左偏移的值都为50%，是元素的左上角在父元素中心点的位置。效果：

[![img](https://img2.mukewang.com/5e967c3d0001fbbf25600616.jpg)](https://img2.mukewang.com/5e967c3d0001fbbf25600616.jpg)

3、然后再用margin给上和左都给负的自身宽高的一半,就能达到垂直水平居中的效果。

#### 面试常考题之宽高不定实现盒子水平垂直居中

这一章我们来学习未知宽高实现盒子水平垂直居中，通常使用定位以及translate完成。参考下面例子：

```html
 <div class="box">
        <div class="box1">
            慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网慕课网
        </div>
    </div>
```

添加样式：

```css
 .box {
        border: 1px solid #00ee00;
        height: 300px;
        position: relative;

    }

    .box1 {
        border: 1px solid red;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
```

效果如下：

[![img](https://img4.mukewang.com/5e967f3b000117da25530461.jpg)](https://img4.mukewang.com/5e967f3b000117da25530461.jpg)

**技术点的解释：**

1、利用父元素设置相对定位,子元素设置绝对定位,那么子元素就是相对于父元素定位的特性。

2、子元素设置上和左偏移的值都为50%。

3、然后再用css3属性translate位移,给上和左都位移-50%距离，就能达到垂直水平居中的效果。