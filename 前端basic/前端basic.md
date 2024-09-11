# html

## 元素分类

### 行内元素

行内元素在同一行中排列。

- `<a>` - 超链接
- `<b>`, `<strong>` - 加粗文本
- `<i>`, `<em>` - 斜体文本
- `<span>` - 通用行内容器
- `<img>` - 图片
- `<input>` - 输入框
- `<label>` - 表单控件标签
- `<select>` - 下拉选择框
- `<textarea>` - 多行文本输入框
- `<code>` - 代码片段
- `<sub>`, `<sup>` - 下标、上标文本
- `<br>` - 换行符
- `<button>` - 按钮

### 块元素

在页面上独占一行，并且可以包含其他块元素或行内元素。

- `<div>` - 通用块级容器
- `<p>` - 段落
- `<h1>` ~ `<h6>` - 标题
- `<ul>`, `<ol>`, `<li>` - 列表
- `<table>` - 表格
- `<form>` - 表单
- `<hr>` - 水平分割线
- `<pre>` - 预格式化文本块
- `<blockquote>` - 长引用
- `<address>` - 联系方式信息
- `<article>`, `<section>`, `<nav>`, `<aside>`, `<header>`, `<footer>` - HTML5 语义化元素

### 行内块元素

既可以像块元素那样设置宽高，又可以像行内元素那样不独占一行。

- `<img>` - 图片
- `<input>` - 输入框
- `<button>` - 按钮
- `<select>` - 下拉选择框
- `<textarea>` - 多行文本框

1. 和相邻行内块元素再一行显示，但是他们之间会有空白缝隙。一行上可显示多个。
2. 默认宽度就是它本身内容的宽度。
3. 高度，宽度，行高，内边距，外边距都可以自己控制。

4. 在布局上表现得像一个行内元素(如文本),但同时也能像块级元素一样设置宽度、高度等属性。





# css

## 属性

###  height: 100vh

 height: 100vh; /* 设置容器高度为视口高度 */



### font

#### font-weight (字体粗细设置)

font-weight: normal / bold / bolder / lighter / 数字值(从100到900的整数) 



### transform 形变

#### 位移 translate(x,y)

```css
.d1 {
    width: 100px;
    height: 100px;
    background-color: aqua;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
```



#### 缩放 scale(x,y)

- 可以改变元素的大小
- 一个值为x轴的缩放
- 二个值为x轴和y轴的缩放
- 数字：1表示不变，0.5表示缩小一班，2表示放大一倍
- 百分比，不常用，也可以设置

```css
.d1 {
    width: 100px;
    height: 100px;
    background-color: aqua;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    transform: scale(0.5,0.5);
}
```



#### 旋转 rotate(角度)

- 旋转：rotate

  - 一个值，表示旋转的角度
  - 常用单位为deg表示角度，一个圆为360deg
  - 正数为顺时针旋转
  - 负数为逆时针旋转
  - 也可以使用其他单位

  *90deg = 100grad = 0.25turn = 1.5708rad*

```css
.d1 {
    width: 100px;
    height: 100px;
    background-color: aqua;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    transform: rotate(45deg);
}
```









## display : table

```html
<style>
  .table {
    display: table;
    border: 1px solid #cccccc;
    margin: 5px;
  }

  .row {
    display: table-row;
    border: 1px solid #cccccc;
  }

  .cell {
    display: table-cell;
    border: 1px solid #cccccc;
    padding: 5px;
  }
</style>

<div class="row">
  <div class="cell">张三</div>
  <div class="cell">李四</div>
  <div class="cell">王五</div>
</div>
<div class="cell">张三</div>
<div class="cell">李四</div>
<div class="cell">王五</div>

```











## 盒模型

### 盒模型宽度计算

计算div1的`offsetWidth`

```html
<style>
    #div1{
        width: 100px;
        padding: 10px;
        border: 1px solid #ccc;
        margin: 10px;
    }
</style>

<div id="div1">    
</div>
```

offsetWidth = (width + padding + border)  = (100px + 10px *2 + 1px * 2) = 122px

#### offsetWidth

offsetWidth =（内容宽度+内边距+边框） 无外边距

offsetWidth = (width + padding + border)  无margin



### margin

#### margin纵向重叠



#### margin负值



#### margin: auto



### box-sizing

以特定的方式定义匹配某个区域的特定元素

默认值: content-box

#### content-box

```css
box-sizing:  content-box
```

padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding)

#### border-box

```css
box-sizing:  border-box
```

padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width )

offsetWidth等于width

#### inherit

规定从父元素继承此值。继承父亲box-sizing 属性值

```css
box-sizing: inherit
```

## 函数

### attr

用于获取所选元素的属性值并将其用作样式的内容。它通常与 `content` 属性一起使用，可以在伪元素中显示元素的属性值

```css
<style>
  a::after {
    content: " (" attr(href) ")";
  }
</style>

<a href="https://www.example.com">链接到 Example 网站</a>
```

- `content: " (" attr(href) ")";` 将伪元素的内容设置为括号中的链接 URL。这里，`attr(href)` 获取了 `<a>` 元素的 `href` 属性的值。

## img

### 当src为空时，alt显示其他内容

```css
img::before {
    content: attr(alt);
    display: inline-block;
    width: 90px;
    height: 90px;
    line-height: 90px;
    text-align: center;
    background-color: aqua;
    color: black;
    font-size: 14px;
    font-weight: bold;
}
```

- `content: attr(alt);` 将 `alt` 属性的值设置为伪元素的内容。
- `display: inline-block;` 确保伪元素与图片在同一行。
- `width` 和 `height` 与图片的尺寸相同。
- `line-height` 与 `height` 相同，以垂直居中文本。
- `text-align: center;` 水平居中文本。
- `background-color: blue;` 设置蓝色背景。
- `color: black;` 设置黑色文本。
- `font-size` 和 `font-weight` 设置字体大小和粗细。





## 布局

### flex布局

#### flex:1

等同于: flex: 1 1 0

**flex: 1** 实际上是三个属性的缩写：**flex-grow: 1; flex-shrink: 1 flex-basis: auto;**

#### flex-grow

这个属性规定了 flex-grow 项在 flex 容器中分配剩余空间的相对比例。 主尺寸是项的宽度或高度，剩余空间是 flex 容器的大小减去所有 flex 项的大小加起来的大小。如果所有的兄弟项目都有相同的 flex-grow 系数，那么所有的项目将剩余空间按相同比例分配，否则将根据不同的 flex-grow 定义的比例进行分配。

- 属性值：`负值无效，默认为 0`

- 公式就是：`原始宽度 + （剩余空间 / 总共分成多少份 * 当前元素所占 分数）`



#### flex-shrink

flex-shrink 属性指定了 flex 元素的`收缩规则`。flex 元素仅在默认宽度之和大于容器的时候才会发生收缩，其收缩的大小是依据 flex-shrink 的值。

- 属性值：`负值无效，默认值为 1`

- 公式就是：

  `缩小比例 = (flex item的flex-shrink值* flex item 的基准宽度)/所有 flex item 的 (flex-shrink 值 * 基准宽度)之和`

```vue
<template>
    <div class="d1">
        <div class="left">left</div>
        <div class="center">center</div>
        <div class="right">right</div>
    </div>
</template>

<script setup lang="ts">

</script>

<style lang="scss" scoped>
.d1{
    display: flex;
    flex-direction: row;
    margin-top:30px;
    width:500px;

}
.d1>div{
    height:100px;
    width:200px;
}
.left{
    background-color: yellow;
    flex-shrink:1;
}
.center{
    background-color: darkcyan;
    flex-shrink:1;
}
.right{
    background-color: red;
    flex-shrink:2;
}
</style>
```

外层div 500px,内层三个 div 每个200px。

flex-shrink分别是 1、1、2

计算步骤:

1. 计算内层 div 的总宽度:
   200px + 200px + 200px = 600px
2. 计算超出的宽度:
   600px - 500px = 100px
3. 计算每个 div 的 (flex-shrink * 基准宽度) 值:
   - .left: 1 * 200 = 200
   - .center: 1 * 200 = 200
   - .right: 2 * 200 = 400
4. 计算 (flex-shrink * 基准宽度) 的总和:
   200 + 200 + 400 = 800
5. 计算每个 div 的缩小比例:
   - .left 的缩小比例 = 200 / 800 = 0.25
   - .center 的缩小比例 = 200 / 800 = 0.25
   - .right 的缩小比例 = 400 / 800 = 0.5
6. 计算每个 div 需要缩小的宽度:
   - .left 需要缩小的宽度 = 100px * 0.25 = 25px
   - .center 需要缩小的宽度 = 100px * 0.25 = 25px
   - .right 需要缩小的宽度 = 100px * 0.5 = 50px
7. 计算每个 div 的最终宽度:
   - .left 的最终宽度 = 200px - 25px = 175px
   - .center 的最终宽度 = 200px - 25px = 175px
   - .right 的最终宽度 = 200px - 50px = 150px

因此,在这个页面中,每个 div 的最终宽度为:

- .left: 175px
- .center: 175px
- .right: 150px



#### flex-basis

CSS 属性 flex-basis 指定了 flex 元素在主轴方向上的`初始大小`。如果不使用 box-sizing 改变盒模型的话，那么这个属性就决定了 flex 元素的内容盒（content-box）的尺寸。

在 flex item 上,如果同时设置了 flex-basis 和 width,flex-basis 会覆盖 width

flex-basis:  0 / auto / 200px;



### grid布局

#### 父元素属性

##### display: grid | inline-grid

- `grid`：声明一个块级Grid容器，占用整个父容器的宽度。
- `inline-grid`：声明一个内联Grid容器，类似于其他内联元素，可随文本流布局。



##### grid-template-columns 和 grid-template-rows

```css
.grid-container {
  grid-template-columns: [line-name-start] length | percentage | fr | auto ... [line-name-end];
  grid-template-rows: [line-name-start] length | percentage | fr | auto ... [line-name-end];
}
```

- 这两个属性用于定义网格的列和行数以及大小，支持多种单位，包括像素值、百分比、fr（fraction单位，表示网格轨道占总轨道的比例）和auto（根自动调整大小）。括号内的`line-name`可用于命名网格线。

```css
.grid-container {
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  // grid-template-columns: repeat(5,1fr);
  // grid-template-columns: repeat(auto-fill,150px);
  // grid-template-columns: auto auto auto auto auto;  自动调整列宽
  // grid-template-columns: 20% 20% 20% 20% 20%;
  // grid-template-columns: 100px 100px 100px 100px 100px; 像素值
  // grid-template-columns: 1fr 100px 20% auto 1fr; 混合使用
}
```

grid-template-columns: repeat(auto-fill,150px);  // 如果还有宽度就会自动填充

grid-template-columns: repeat(auto-fill,minmax(150px,1fr));    

1. 浏览器首先根据容器的宽度和指定的最小列宽度 `150px` 计算可以容纳的最大列数。
2. 如果容器宽度足够,浏览器会创建尽可能多的列,每列的宽度至少为 `150px`。
3. 如果在满足最小列宽度的前提下还有剩余空间,这些剩余空间会平均分配给每一列,使列的宽度在 `150px` 到 `1fr` 之间动态调整。
4. 当容器宽度变化时,浏览器会自动重新计算列数和列宽,以适应新的容器宽度。



-  `grid-template-columns: repeat(auto-fill, minmax(100px, 1fr))`
  - 如果容器的宽度为 1000 像素,最大可以创建 10 个宽度为 100 像素的列 (1000 / 100 = 10)。
  - 但是,如果实际内容只需要 5 个列,`repeat(auto-fill, ...)` 会创建 5 个列。
  - 在这种情况下,剩余的空间为 500 像素 (1000 - (5 * 100) = 500)。
  - `1fr` 会将这 500 像素的剩余空间平均分配给 5 个列,每个列获得额外的 100 像素宽度 (500 / 5 = 100)。
  - 因此,最终每个列的实际宽度将是 200 像素 (100 + 100),大于最小宽度 100 像素。



##### grid-auto-rows，grid-auto-rows

当网格项目的数量超过显式定义的行数时,浏览器会自动创建额外的行,这些行的大小由 `grid-auto-rows` 属性决定。

默认值为 auto

```css
grid-auto-rows: auto / 10em / 20% / 1fr /minmax(100px,auto) / fit-content(200px) / 100px 50px;
```

`minmax()` 函数: 可以使用 `minmax()` 函数来指定行高的范围。它接受两个参数:最小值和最大值。

`fit-content()` 函数: 可以使用 `fit-content()` 函数来根据内容自适应行的高度,同时设置一个最大高度限制。

多个值: 可以提供多个值,表示不同的行高度模式。网格将循环应用这些值。





##### grid-row-gap，grid-columns-gap，gap

```css
grid-row-gap: 20px;
// 简写
gap: 20px 20px;
gap: 20px; // 行列间距均为20px
```



##### grid-colunm-start/end，grid-row-start/end，grid-row，grid-colunm

定网格项目在网格中的起始和结束位置

<img src="./img/2.jpg" style="width:40%" align="left">

```css
<template>
    <div class="table">
        <div class="header"></div>
        <div class="body1"></div>
        <div class="body=2"></div>
        <div class="footer"></div>
    </div>
</template>

<script setup lang="ts"></script>

<style lang="scss" scoped>
.table{
    margin-top:10px;
    display: grid;
    grid-template-rows:40px 300px 40px;
    grid-template-columns:300px 300px;
    gap:30px;
    div{
        background-color:gold;
        border: 3px solid aqua;
        border-radius: 10px;
    }
}

.header{
    grid-column-start:1;
    grid-column-end:3;
    grid-row-start: 1;
    grid-row-end:2;
}
.footer{
    grid-column-start:1;
    grid-column-end:3;
    grid-row-start: 3;
    grid-row-end:4;
}
</style>
```



```css
.header{
    grid-column-start:1;
    grid-column-end:3;
    grid-row-start: 1;
    grid-row-end:2;
}
// 等同于
.header {
    grid-column: 1 / 3;
    grid-row: 1 / 2;
}
```

<img src="./img/3.jpg" style="width:40%" align="left">



##### grid-template-areas

使用命名的网格区域来定义网格布局

```vue
<template>
    <div class="table">
        <div class="header"></div>
        <div class="body1"></div>
        <div class="body2"></div>
        <div class="footer"></div>
    </div>
</template>

<script setup lang="ts"></script>

<style lang="scss" scoped>
.table {
    margin-top: 10px;
    display: grid;
    grid-template-rows: 40px 300px 40px;
    grid-template-columns: 300px 300px;
    gap: 30px;
    grid-template-areas:
        "header header"
        "body1 body2"
        "footer footer";

    div {
        background-color: gold;
        border: 3px solid aqua;
        border-radius: 10px;
    }
}

.header {
    grid-area: header;
}

.body1 {
    grid-area: body1;
}

.body2 {
    grid-area: body2;
}

.footer {
    grid-area: footer;
}
</style>
```



##### justify-items、align-items 和 place-items

- justify-items：设置单元格内容的水平位置（左中右）

```css
justify-items: auto | normal | stretch | center | start | end | flex-start | flex-end | self-start | self-end | left | right | baseline | first baseline | last baseline | safe center | unsafe center | legacy right | legacy left | legacy center | inherit | initial | unset;

```

- align-items：设置单元格内容的垂直位置（上中下），与 justify-items 类似。

- place-items：是 align-items 和 justify-items 的合并简写形式

##### justify-content、align-content 和 place-content

- justify-content：是整个内容区域在容器里面的水平位置（左中右）

```css
justify-content: center | start | end | flex-start | flex-end | left | right | baseline | first baseline | last baseline | space-between | space-around | space-evenly | stretch | safe center | unsafe center | inherit | initial | unset;
```

- align-content：是整个内容区域的垂直位置（上中下）
- place-content：是 align-content 和 justify-content 的合并简写形式



##### 网格线命名

```css
.container {
    display: grid;
    grid-template-rows: [row1-start] 100px [row1-end row2-start] 200px [row2-end];
    grid-template-columns: [col1-start] 1fr [col1-end col2-start] 1fr [col2-end];
}
// 可以为同一条网格线指定多个名称,使用空格分隔,如 [row1-end row2-start]。

.item1 {
    grid-area: row1-start / col1-start / row1-end / col2-end;
}

.item2 {
    grid-area: row2-start / col1-start / row2-end / col1-end;
}

.item3 {
    grid-area: row2-start / col2-start / row2-end / col2-end;
}
```





##### grid-auto-flow

用于控制网格中自动布局的算法,即当网格项目没有明确指定位置时,浏览器如何自动放置这些项目。



#### 子元素属性

##### grid-area

指定网格项目在网格容器中的位置和跨度

```css
.item {
    grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
// 可以是一个数字,表示网格线的编号;也可以是一个命名的网格线。
```



```css
.item {
    grid-area: 1 / 1 / 3 / 4;
}
```













## 案例

### 盒子页面居中

html给高度100%

1. flex实现

```html
<!DOCTYPE html>
<html lang="en" style="height: 100%;">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body style="display: flex;justify-content: center;align-items: center;height: 100%;width: 100%;">
    <div style="width: 100px;height: 100px;background-color: aqua;"></div>

</body>

</html>
```

2. 绝对定位 

top: 50%, left: 50%

```html
<!DOCTYPE html>
<html lang="en" style="height: 100%;">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body style="height: 100%; width: 100%; margin: 0; position: relative;">
    <div style="width: 100px; height: 100px; background-color: aqua; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);"></div>
</body>
</html>
```

- 首先,通过设置 `top: 50%; left: 50%;`,我们将元素的左上角移动到其父元素(在这个例子中是body)的中心。

- 但是,这只是将元素的左上角对准了中心点。元素自身的中心仍然偏右偏下。

- 为了解决这个问题,我们需要向左和向上移动元素,移动距离分别为其自身宽度和高度的一半。这就是 `translate(-50%, -50%)` 的作用。

- `translate(-50%, -50%)` 使元素在水平和垂直方向上分别移动其自身宽度和高度的一半,但方向相反(因为值是负的)。这effectively使元素自身的中心与其父元素的中心对齐。



3.  grid实现

父元素给 dispaply: grid; justify-content;align-items: center;

```html
<!DOCTYPE html>
<html lang="en" style="height: 100%;">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .d1 {
            width: 100px;
            height: 100px;
            background-color: aqua;
        }
    </style>
</head>

<body style="display: grid;height: 100%; width: 100%; margin: 0;justify-content: center;align-items: center;">
    <div class="d1">
    </div>
</body>

</html>
```





## 选择子元素

.d1 > .d2

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .d1 > .d2 {
            color: red;
            font-size: 20px;
            font-weight: bold;
        }
    </style>
</head>

<body>
    <div>
        <div class="d1">
            <div class="d2">1111</div>
        </div>
    </div>
</body>

</html>
```



# js

## dom

### 事件

#### input事件和change事件区别

- `input`事件在每次值发生变化时都会立即触发，因此它提供了实时的反馈。当你需要实时响应用户的输入并执行相应的操作时，可以使用 `input` 事件。
- `change` 事件只在值发生变化并失去焦点后触发，因此它提供了延迟的反馈。当你需要在用户完成输入后再执行相应的操作时，可以使用 `change` 事件。



## typeof & Object.prototype.toString.call(~)



### Object.prototype.toString.call(~)

`Object.prototype` 是一个对象,它包含了所有 JavaScript 对象共享的属性和方法。

`Object.prototype.toString.call(undefined)`：

- `call` 不是 string 类型的方法，而是 `Function.prototype` 上的方法。所有函数都可以访问 `call` 方法。
- `Object.prototype.toString` 是一个函数，所以它可以使用 `call` 方法。
- `call` 方法允许你为函数指定 `this` 值。在这个例子中，`undefined` 被传递给 `call`，所以 `toString` 方法内部的 `this` 值将是 `undefined`。
- `Object.prototype.toString.call(undefined)` 将返回字符串 `"[object Undefined]"`。





## prototype 属性

每个函数创建的时候都会**自动创建一个prototype属性**，**prototype属性是函数独有的**。prototype的含义是函数的`原型对象`，也就是这个函数（其实所有函数都可以作为构造函数）所创建的实例的原型对象

Object.prototype 属性表示 Object 的原型对象。

```javascript
function Person(){} 
let person1 = new Person()
由此可知
person1.__proto__ === Person.prototype  // true 它们两个指向完全一样
```



以下是一些常见的内置对象的 `prototype`:

1. `Function.prototype`: 所有函数的原型对象,提供了如 `apply()`, `call()`, `bind()` 等方法。
2. `Array.prototype`: 所有数组的原型对象,提供了如 `push()`, `pop()`, `slice()`, `map()` 等方法。
3. `String.prototype`: 所有字符串的原型对象,提供了如 `charAt()`, `slice()`, `toLowerCase()`, `trim()` 等方法。
4. `Number.prototype`: 所有数字的原型对象,提供了如 `toFixed()`, `toPrecision()`, `toString()` 等方法。
5. `Boolean.prototype`: 所有布尔值的原型对象,提供了如 `toString()`, `valueOf()` 等方法。
6. `Date.prototype`: 所有日期对象的原型对象,提供了如 `getFullYear()`, `setMonth()`, `toLocaleString()` 等方法。
7. `RegExp.prototype`: 所有正则表达式对象的原型对象,提供了如 `test()`, `exec()` 等方法。



### Object.prototype

**属性**

Object.prototype.constructor
特定的函数，用于创建一个对象的原型。

Object.prototype.proto
指向当对象被实例化的时候，用作原型的对象。

Object.prototype.noSuchMethod
当未定义的对象成员被调用作方法的时候，允许定义并执行的函数。

Object.prototype.count
用于直接返回用户定义的对象中可数的属性的数量。已被废除。

Object.prototype.parent
用于指向对象的内容。已被废除。

**方法**
Object.prototype.defineGetter()
关联一个函数到一个属性。访问该函数时，执行该函数并返回其返回值。

Object.prototype.defineSetter()
关联一个函数到一个属性。设置该函数时，执行该修改属性的函数。

Object.prototype.lookupGetter()
返回使用 defineGetter 定义的方法函数 。

Object.prototype.lookupSetter()
返回使用 defineSetter 定义的方法函数。

Object.prototype.hasOwnProperty()
返回一个布尔值 ，表示某个对象是否含有指定的属性，而且此属性非原型链继承的。

Object.prototype.isPrototypeOf()
返回一个布尔值，表示指定的对象是否在本对象的原型链中。

Object.prototype.propertyIsEnumerable()
判断指定属性是否可枚举，内部属性设置参见 ECMAScript [[Enumerable]] attribute 。

Object.prototype.toSource()
返回字符串表示此对象的源代码形式，可以使用此字符串生成一个新的相同的对象。

Object.prototype.toLocaleString()
直接调用 toString()方法。

Object.prototype.toString()
返回对象的字符串表示。

Object.prototype.unwatch()
移除对象某个属性的监听。

Object.prototype.valueOf()
返回指定对象的原始值。

Object.prototype.watch()
给对象的某个属性增加监听。

## `__proto__`



## constructor



### 区别

1. 判断的类型范围不同:
   - `typeof` 可以判断出 `"undefined"`, `"boolean"`, `"number"`, `"string"`, `"symbol"`, `"function"` 和 `"object"` 这7种类型。
   - `Object.prototype.toString.call()` 可以判断出更多的类型,包括 `"[object Undefined]"`, `"[object Null]"`, `"[object Boolean]"`, `"[object Number]"`, `"[object String]"`, `"[object Symbol]"`, `"[object Function]"`, `"[object Array]"`, `"[object Date]"`, `"[object RegExp]"`, `"[object Object]"` 等。
2. 对 `null` 的判断不同:
   - `typeof null` 返回 `"object"`,这是 JavaScript 的一个历史遗留 bug。
   - `Object.prototype.toString.call(null)` 返回 `"[object Null]"`,可以正确判断 `null`。
3. 对对象的判断不同:
   - `typeof` 对所有的对象类型(包括数组,日期等)都返回 `"object"`,无法区分它们。
   - `Object.prototype.toString.call()` 可以区分不同的对象类型,如 `"[object Array]"`, `"[object Date]"` 等。
4. 调用方式不同:
   - `typeof` 是一个操作符,后面直接跟要判断类型的值,如 `typeof 123`, `typeof "abc"` 等。
   - `Object.prototype.toString.call()` 是一个方法,需要以 `call()` 的形式调用,并将要判断类型的值作为第一个参数传入。



## 函数当做对象使用

```javascript
// 定义一个函数 greet
function greet(name) {
  return `Hello, ${name}!`;
}

// 给 greet 函数添加属性 language 和 method
greet.language = 'English';
greet.method = function () {
  return 'Greeting';
};

// 调用 greet 函数
console.log(greet('John')); // 输出: Hello, John!

// 访问 greet 函数的属性和方法
console.log(greet.language); // 输出: English
console.log(greet.method()); // 输出: Greeting
```



## 时间

### Date



### 当前时间

```typescript
function formatDate(date: Date) {
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    return `${year}-${month}-${day} ${hours}:${minutes}`;
}

const currentTime = ref(new Date());
<div>当前时间: {formatDate(currentTime.value)}</div>
```



##  == 和 === 的区别

相等运算符(==)会进行类型的转换

全等运算符(===)不会进行类型的转换

```js
6 == "6" // true  自动类型转换
6 === "6" // false

true == 1; // true
false == 0; // true
true === 1; // false
false === 0; // false

'' == 0 // true
' ' == 0 // true
null == undefined // true
null == 0 // false
undefined == '' //false 

'false' == false // false  false会被转为0
'0' == false  // true
NaN == NaN // false
NaN == false // false
NaN === false // false
```

- NaN

not a number



```js
var a = {}
var b = {}
var c = a;

a == b; // false
a === b; // false
a == c; // true
a === c; //true
```

<img  src="./img/1.jpg" align="left" style="width:30%">



#### NaN

NaN 是 JavaScript 中的一个特殊值，表示 "Not-a-Number"（非数值）。当你尝试对非数值类型的值进行数学运算时，或者当计算结果无法表示为数值时，就会出现 NaN。

```js
// 1.对非数值类型进行算术运算
"abc" * 2;  // NaN
undefined + 1;  // NaN

// 2.对非法的数学表达式求值
Math.sqrt(-1);  // NaN
parseFloat("abc");  // NaN

// 3.特殊的函数调用
Math.sqrt(-1);  // NaN
parseFloat("abc");  // NaN

// 4.NaN 有一个特殊的属性，即它不等于自身
NaN === NaN;  // false

// 5.要判断一个值是否为 NaN，应该使用 isNaN() 函数
isNaN(NaN);  // true
isNaN("abc");  // true
isNaN(123);  // false
```







## es6

### 相关名词

#### ECMA

ECMA国际 (Ecma international) 前身为 欧洲计算机制造商协会 ECMA（European Computer Manufacturers Association）

#### ECMAScript

由 ECMA国际 通过 ECMA-262 标准化的脚本设计语言



### es6模块化

在es6模块化诞生之前，javascript社区已经尝试并提出了AMD、CMD、CommonJS等模块化规范

- AMD和CMD适用于浏览器端的 javascript模块化
- CommonJS适用于服务器端的 Javascript模块化

#### `CommonJs规范`

node.js遵循了CommonJS的模块化规范，其中：

- 导入其他模块使用requier() 方法
- 模块对外共享成员使用module.exports对象



#### es6模块化规范

ES6模块化规范是浏览器端和服务器端通用的模块化开发规范

- 每个 js 文件都是一个独立的模块
- 导入其他模块成员使用 import 关键字
- 向外共享模块成员使用 export 关键字



### 拓展运算符

1. 展开操作符

```js
const arr = [1,2,3,4]
const arr2 = [...arr]
// arr2 [1,2,3,4]
```

```typescript
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // arr2 将是 [1, 2, 3, 4, 5]
```

2. 对象的属性展开

```typescript
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // obj2 将是 { a: 1, b: 2, c: 3 }
```

3. 解构赋值

```typescript
const [first, ...rest] = [1, 2, 3, 4]; // first 将是 1，rest 将是 [2, 3, 4]
const { a, ...restProps } = { a: 1, b: 2, c: 3 }; // a 将是 1，restProps 将是 { b: 2, c: 3 }
```



```js
const arr = [1,2,3]
const arr1 = [4,5,6]
const res = [...arr,...arr1]
console.log(res)   // [1,2,3,4,5,6]
```

```js
const obj = {name:'li'}
const obj1 = {age:20}
const res = Object.assign(obj,obj1)
// 等同于
// const res = {...obj,...obj1}
```

```typescript
type execFn = (...args: any[]) => void

const logArguments: execFn = (...args: any[]) => {
  console.log(args);
};

logArguments(1, 'hello', true); // 输出：[1, 'hello', true]
logArguments('a', 'b', 'c'); // 输出：['a', 'b', 'c']
```





```vue
const itemShare = {
      labelWidth: "120px",
};


function share(query: ICurrentFromQuery): object {
  return {
    ref: setRef(query.prop),
    class: "form_item",
    modelValue: form[query.prop],
    "onUpdate:modelValue": (e: any) => {
      form[query.prop] = e;
    },
  };
}

<LdFormItem
	{...itemShare}
    label="单号"
    prop={getProp("dh")}
    v-slots={{
    	default: (query: ICurrentFromQuery) => {
    		return (
                <>
                  <ElInput
                    {...share(query)}
                    style={{ width: "220px" }}
                    placeholder="输入单号查询"
                    onKeydown={(e) => {
                      if (isEnter(e)) {
                        handleQuery();
                      }
                    }}
                  ></ElInput>
                </>
            );
        },
    }}
/>
```



### 箭头函数

参数只有一个或者return 一个元素可以省略return

```js
const obj = {
   name: 'zhang',
   sayName: function () {
   console.log(this.name);
   },
}
```

```js
const obj1 = {
    name: 'zhang',
    sayName: () => {
        console.log(obj.name);
    },
}
```

箭头函数不能当做构造函数，箭头函数没有自己的this

箭头函数没有arguments对象



### 可选链 ?. 运算符

允许你在访问对象的属性或调用对象的方法时，如果对象的某个部分是 `null` 或 `undefined`，运算会短路并返回 `undefined` 而不会继续访问后续属性以及抛出错误。

- 基本语法

```js
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
```

- 访问对象的嵌套属性

```js
let user = {
    profile: {
        name: "Alice",
        address: {
            city: "Wonderland"
        }
    }
};

// 使用可选链访问嵌套属性
let city = user?.profile?.address?.city;
console.log(city); // 输出 "Wonderland"

// 如果某个属性不存在，不会抛出错误
let country = user?.profile?.address?.country;
console.log(country); // 输出 undefined
```

- 访问数组元素

```js
let arr = [1, 2, 3];

// 使用可选链访问数组元素
let firstElement = arr?.[0];
console.log(firstElement); // 输出 1

// 访问超出范围的索引，不会抛出错误
let outOfRangeElement = arr?.[10];
console.log(outOfRangeElement); // 输出 undefined
```

- 调用对象的方法

```javascript
let user = {
    greet: function() {
        return "Hello!";
    }
};

// 使用可选链调用方法
let greeting = user?.greet?.();
console.log(greeting); // 输出 "Hello!"

// 调用不存在的方法，不会抛出错误
let farewell = user?.farewell?.();
console.log(farewell); // 输出 undefined
```



### 空位合并 ?? 运算符

`??空位合并运算符`:当左侧操作数为 null 或 undefined 时，其返回右侧的操作数。否则返回左侧的操作数。

```javascript
const a = 0 || 'X'   // X
const b = '' || 'X'   // X
const c = false || 'X'   // X
const d = undefined || 'X'   // X
const e = null || 'X'   // X

//若想使||前的 0 ''  false 均为真 则需要使用空位合并运算符 ?? 
//空位合并运算符 ??
const a = 0 ?? 'X' // 0
const b = '' ?? 'X' // ''
const c = false ?? 'X' // false
const d = undefined ?? 'X' // X
const e = null ?? 'X' // X

```







### 求幂运算符

```js
const num = Math.pow(4,5) // old
const num1 = 4 ** 5 // new
```



### 解构赋值

#### 数组解构赋值

```js
const [a, b] = [1, 2];
console.log(a); // 输出 1
console.log(b); // 输出 2
```

```js
let a = 1;
let b = 2;
[a, b] = [b, a]
// 不添加新的变量的情况下交换a，b
```



#### 对象解构赋值

```js
const obj = {
    nickname: "小李",
    age: 30
}
const { nickname, age } = obj
```

```js
const { x, y } = { x: 1, y: 2 };
console.log(x); // 输出 1
console.log(y); // 输出 2
```

```js
const { x: x1, y: y1 } = { x: 1, y: 2 };
```

```js
const { sourceProperty: targetVariable } = object;
// sourceProperty: 要从对象中提取的属性名
// targetVariable：要将属性值赋给的目标变量名
```



- 对象嵌套解构赋值

```js
const person = {
  name: 'John',
  age: 30,
  address: {
    city: 'New York',
    country: 'USA'
  }
};

const { name, address: { city, country } } = person;

console.log(name);    // 输出: John
console.log(city);    // 输出: New York
console.log(country); // 输出: USA
```

```js
const obj = {
    nickname: "小李",
    age: 30,
    doing: {
        morning: "起床",
        evening: "睡觉"
    }
}
const { nickname, age, doing: { morning: m1, evening: e1 } } = obj
console.log(m1, e1);
```

```js
const obj = {
    nickname: "小李",
    age: 30,
    doing: {
        morning: "起床",
        evening: "睡觉"
    }
}
const { nickname, age, doing } = obj
console.log(doing);
// {morning: '起床', evening: '睡觉'}
```



#### 乱序结构
```js
const arr = [1, 2, 3]
const { 1: a, 2: b, 0: c } = arr
console.log(a, b, c)
// 2 3 1
```

#### 使用案例

```js
const arr = [1, 2, 3, 4, 5];
const [, , num3, , num5] = arr;
```

```js
const arr=[1,2,3,4,5]
const [num1,num2,num3]=arr
```

```js
const arr = [1, 2, 3, 4, 5]
const [ ...Nums] = arr;  // 拷贝数组
```

```js
const arr = [1, 2, 3, 4, 5]
const [n1, n2, n3, ...restNum] = arr;
```

```ts
const arr = [1, 2, [3, 4], [5, 6, 7]] as any[];
const [n1, , [, n2], [, n3,]] = arr;
```

```js
const arr = ['name','email'];
const {[arr[0]]:eggName,[arr[1]:eggEmail]}
```

```js
const obj = {
    id: '01',
    name: 'li',
    age: 18,
    gender: '男',
    email: '123@qq.com'
}
const arr = ['name', 'email'];

const { [arr[0]]: eggName, [arr[1]]: eggEmail } = obj;

console.log(eggName, eggEmail) 
```

```js
const obj = {
    id: '1',
    name: 'li'
}

const { name } = obj

// -------------------

const obj = {
    id: '1',
    name: 'li'
}

let name = 'wang';

({ name } = obj);

console.log(name);
// 解构赋值覆盖变量
```

```js
const arr = [
    { name: 'li', age: 10 },
    { name: 'wang', age: 15 },
    { name: 'zhang', age: 20 }
]

arr.forEach(({ name, age }, index) => {
    console.log(`姓名:${name},年龄:${age}`);
})
```



### 对象的简化写法

```js
let name = 'li'
let talk = function(){
    console.log("hello world")
}

const person ={
    name,
    talk,
    age:10
}
```



```javascript
const fish = {  
  swim() {
    console.log("Fish is swimming");
  }
};

// 等同于
const fish = {
  swim: function() {
    console.log("Fish is swimming");
  }
};
```





### rest参数

- js 可以不传递参数给有参数的函数，参数值为undefined

- js 可以给没有参数的函数传递参数
- ts不行

#### arguments

```js
// es5 使用arguments获得实参
function date(){
    console.log(arguments)
}

date('a','b','c')
```



#### rest

```js
function date(...args){
    console.log(args)
}

function date(a,b,...args){
    console.log(a)
    console.log(b)
    console.log(args)
}
```



### Symbol

ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。

Symbol 值通过`Symbol()`函数生成。

对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

- Symbol 的值是唯一的，用来解决命名冲突的问题
- Symbol 值不能与其他数据进行运算
- Symbol 定义的对象属性不能使用for...in 循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名

```js
let s = Symbol();
console.log(s,typeof s);
// Symbol()   "symbol"

let s2 = Symbol('hello')
let s3 = Symbol('hello')
console.log(s2===s3)  // false

// 不能与其他数据进行运算
let result = s + "1" // 报错

let s4 = Symbol.for("hello")
let s5 = Symbol.for("hello")
console.log(s4 === s5) // true
```



1. `Symbol('hello')`：
   - 每次调用 `Symbol('hello')` 都会创建一个新的唯一的 Symbol 值，即使传入的字符串参数相同。
   - 通过 `Symbol('hello')` 创建的 Symbol 值不会被注册到全局 Symbol 注册表中。
   - 不同的 `Symbol('hello')` 创建的 Symbol 值是不相等的，即使它们的字符串参数相同。

2. `Symbol.for('hello')`：
   - `Symbol.for('hello')` 会首先在全局 Symbol 注册表中查找键为 `'hello'` 的 Symbol 值。
   - 如果找到了对应的 Symbol 值，就直接返回该 Symbol 值。
   - 如果没有找到，则会创建一个新的 Symbol 值，并将其注册到全局 Symbol 注册表中，然后返回该 Symbol 值。
   - 后续再次调用 `Symbol.for('hello')` 会直接从全局 Symbol 注册表中获取对应的 Symbol 值，而不会创建新的 Symbol 值。
   - 通过 `Symbol.for('hello')` 创建的 Symbol 值在全局范围内是唯一的，即使在不同的代码文件或模块中使用相同的字符串参数调用 `Symbol.for()`，返回的 Symbol 值也是相同的。





### for in / for of

<span style="color:red">for…of输出的是数组的值，for…in输出的数组的索引，并且输出的索引是字符串</span>

对象无法使用for of

#### for...in

```js
const arr = [3,2,1]
for(let index in arr){
    console.log(arr[index])  
}   // 3,2,1

const obj = {
    name:'li',
    age:10,
    gender:'male'
}
for(let key in obj){
    console.log(obj[key])
}
```

对象使用for in 输出的是对象的key



#### for...of

在JavaScript中可迭代对象被认为是可以在for...of循环中使用的对象。

```js
const arr = [3,2,1]
for(let value of arr){
    console.log(value)
} // 3,2,1

const obj = {
    name:'li',
    age:10,
    gender:'male'
}
for(let value of obj){
    console.log(value)
}  // 报错：obj不是可迭代对象
```



#### 可迭代对象

数组、Set、Map、字符串

```js
let str1 = 'hello'
    for (let s of str1) {
        console.log(s);
}
```





### find / findIndex

返回第一个匹配条件的元素或索引

- find

```js
const person = [
    { name: 'li', age: 25 },
    { name: 'wang', age: 36 },
    { name: 'zhang', age: 47 }
]

const index = person.find(item => item.name === 'wang')  // { name: 'wang', age: 36 }
const index1 = person.find((item) => {
    return item.name === 'wang'  
})
```



- findIndex

```js
const person = [
    { name: 'li', age: 25 },
    { name: 'wang', age: 36 },
    { name: 'zhang', age: 47 }
]

const index = person.findIndex(item => item.name === 'wang')  //1
const index1 = person.findIndex((item) => {  //1
    return item.name === 'wang'  
})

```



### includes/indexOf

字符串/数组使用,判断是否含有某个元素

```js
const str = "hello world"
const bol = str.includes("world")  // true

const arr = [1, 2, 3, 4, 5]
const bol1 = arr.includes(6)  // false
```

```js
const arr = [1, 2, 3, 4, 5]
const bol1 = arr.indexOf(6);  // -1
// index无法判断数组是否含有NaN
```



## 字符串方法

###  str.length

获取长度

```js
const str = "hello",
str.length
```

###   str.charAt(~)  str.charAtCode(~)  

获取字符串指定位置的值 

charCodeAt()：该方法会返回指定索引位置字符的 Unicode 值，返回值是 0 - 65535 之间的整数

```js
const str = 'hello';
str.charAt(1)  // 输出结果：e 

let str = "abcdefg";
console.log(str.charCodeAt(1)); // "b" --> 98
```

### str.split(~)

分割字符串

```js
let str = "Hello";
let s = str.split("e");
console.log(str); //Hello
console.log(s); //[ 'H', 'llo' ]
```

### slice(start, end)

提取字符串某个部分

```js
let str = "Hello";
let s = str.slice(1, 2);
console.log(s); //e
```

```tsx
const str = "hello"
let s = str.slice(2);
console.log(s); //llo
```

拷贝数组

```js
const str = "hello"
let s = str.slice();
console.log(s); // hello
```



### str.padStart(~)

在字符串的开头填充另一个字符串,直到结果字符串达到给定的长度

```javascript
'abc'.padStart(5, 'x');  // 'xxabc'
'abc'.padStart(5, '0');  // '00abc'

// date.getMonth() 返回一个表示月份的数字,范围从0到11
String(date.getMonth() + 1).padStart(2, '0') // 如果月份为1，会变成01
```



### a.localeCompare(b) 

比较字符串字母顺序

```js
let res = titleA.localeCompare(titleB);
```

- 如果 `titleA` 在字母顺序上小于 `titleB`，则返回负数。
- 如果 `titleA` 在字母顺序上大于 `titleB`，则返回正数。
- 如果 `titleA` 和 `titleB` 在字母顺序上相等，则返回零。



## 数组方法

### arr.push(~)   

向数组末尾添加

```js
const arr = [];
arr.push(3);
arr.push(2);
arr.push(1);
// [3, 2, 1]
```

### arr.unshift(~)  

将元素插入到数组的起始位置，并将其他元素向后移动

```js
const arr = [];
arr.unshift(3);
arr.unshift(2);
arr.unshift(1);
// [1,2,3]
```

### arr.include(~) 

判断是否有某个元素

```js
const num1: number[] = [1, 2, 3, 4, 5];
console.log(num1.includes(1)); // 输出: true
console.log(num1.includes(6)); // 输出: false
```

### arr.sort(~)

数组排序

在 `sort()` 方法中，比较函数的返回值决定了元素的排序顺序：

- 如果比较函数返回一个负数，则表示第一个元素应该排在第二个元素之前。
- 如果比较函数返回一个正数，则表示第一个元素应该排在第二个元素之后。
- 如果比较函数返回零，则表示两个元素的顺序相等，不需要交换它们的位置。

```ts
// 1.对数字数组进行升序排序:
const numbers: number[] = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];
numbers.sort((a, b) => a - b);
console.log(numbers); // 输出: [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]

// 2.对数字数组进行降序排序:
const numbers: number[] = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];
numbers.sort((a, b) => b - a);
console.log(numbers); // 输出: [9, 6, 5, 5, 5, 4, 3, 3, 2, 1, 1]

// 3.对字符串数组进行字母顺序排序:
const fruits: string[] = ['banana', 'apple', 'orange', 'grape'];
fruits.sort();
console.log(fruits); // 输出: ['apple', 'banana', 'grape', 'orange']
```

### arr.forEach(~)

```js
const arr = ['a', 'b', 'c', 'd', 'e']

arr.forEach((item, index, arr) => {
    console.log(item, index, arr) // 元素，索引，数组本身
})
// a 0 [ 'a', 'b', 'c', 'd', 'e' ]
// b 1 [ 'a', 'b', 'c', 'd', 'e' ]
// c 2 [ 'a', 'b', 'c', 'd', 'e' ]
// d 3 [ 'a', 'b', 'c', 'd', 'e' ]
// e 4 [ 'a', 'b', 'c', 'd', 'e' ]
```

### arr.map(~)

```js
const arr = [1, 2, 3, 4, 5]
const newArr = arr.map(item => item * 2)

arr.map((item,index,arr)=>{
    console.log(item,index,arr)
})
```

- `map()` 方法会返回一个新的数组,该数组的元素是原始数组中的每个元素调用回调函数的结果。
- `forEach()` 方法没有返回值,它只是对数组中的每个元素执行回调函数,不会生成新的数组。

### arr.filter(~)

```js
const arr = [1, 2, 3, 4, 5]
const newArr = arr.filter(item => item % 2 === 0)  // [2,4]
```

### arr.some(~)/arr.every(~)

```js
const arr = [1, 2, 3, 4, 5]

const flag = arr.some(item => item > 3)    // true
const flag1 = arr.every(item => item > 3)  //false
```

### arr.reduce(~)

pre: 上一次调用的返回值

next: 数组中当前被处理的元素

```js
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((pre, next) => {
    return pre + next;  // 累加
}) // 15
```

```js
const arr1 = [1, 1, 2, 3, 4, 5, 5]
const res = []
arr1.reduce((pre, next) => {
    if (!pre.get(next)) {
        pre.set(next, 1)
        res.push(next)
    }
    return pre
}, new Map())

// pre第一次调用时为初始值 new Map()

```





## Object方法

### Object.keys()

- old

```js
const obj = { name: 'zhangsan', age: 20 };
const keys = Object.getOwnPropertyNames(obj);  // [ 'name', 'age' ]

for (let key of keys) {
    console.log(obj[key]);
} 
// zhangsan 
// 20


const obj = { name: 'zhangsan', age: 20 };

for (let key in obj) {
    console.log(obj[key]);
}
```

- new

```js
const obj = { name: 'zhangsan', age: 20 };


const keys = Object.keys(obj);  // [ 'name', 'age' ]

for (let key of keys) {
    console.log(obj[key]);
}
// 与Object.getOwnPropertyNames相同
// zhangsan 
// 20
```

### Object.values(~)

把对象的所有属性值写成一个数组

```js
const obj = {
    name: 'li',
    age: 20,
    gender: 'male'
}

const values = Object.values(obj);  // [ 'li', 20, 'male' ]
```

## trailing comma

在 JavaScript 中，在数组或对象的最后一项后面添加一个逗号（trailing comma）是被允许的，并且不会导致语法错误。这种语法被称为 "trailing comma" 或 "final comma"

```javascript
const arr = [{
    width: 120,
    dataKey: "STATUS_MC",
    title: "状态",
    filter: true,
},]
```



## this指向

### 理解this

- 常见面向对象的编程语言中，比如Java、C++、Swift、Dart等等一系列语言中，this通常只会出现在`类的方法`中。
- 也就是你需要有一个类，类中的方法（特别是实例方法）中，this代表的是当前调用对象。
- 但是JavaScript中的this更加灵活，无论是它出现的位置还是它代表的含义。



如果没有this，那么我们的代码会是下面的写法：

- 在方法中，为了能够获取到name名称，必须通过obj的引用（变量名称）来获取。
- 但是这样做有一个很大的弊端：如果我将obj的名称换成了info，那么所有的方法中的obj都需要换成info。

```javascript
var obj = {
  name: "why",
  running: function() {
    console.log(obj.name + " running");
  },
  eating: function() {
    console.log(obj.name + " eating");
  },
  studying: function() {
    console.log(obj.name + " studying");
  }
}
```

- 当我们通过obj去调用running、eating、studying这些方法时，this就是指向的obj对象

```javascript
var obj = {
  name: "why",
  running: function() {
    console.log(this.name + " running");
  },
  eating: function() {
    console.log(this.name + " eating");
  },
  studying: function() {
    console.log(this.name + " studying");
  }
}
```



### this指向什么

在全局作用域下，我们可以认为this就是指向的window

```javascript
console.log(this); // window

var name = "why";
console.log(this.name); // why
console.log(window.name); // why
```



```javascript
// 定义一个函数
function foo() {
  console.log(this);
}

// 1.调用方式一: 直接调用
foo(); // window

// 2.调用方式二: 将foo放到一个对象中,再调用
var obj = {
  name: "why",
  foo: foo
}
obj.foo() // obj对象

// 3.调用方式三: 通过call/apply调用
foo.call("abc"); // String {"abc"}对象
```



- 1.函数在调用时，JavaScript会默认给this绑定一个值；
- 2.this的绑定和定义的位置（编写的位置）没有关系；
- 3.this的绑定和调用方式以及调用的位置有关系；
- 4.this是在运行时被绑定的；



```javascript
// 定义一个函数
function foo() {
  console.log(this);
}

// 1.调用方式一: 直接调用
foo(); // window

// 2.调用方式二: 将foo放到一个对象中,再调用
var obj = {
  name: "why",
  foo: foo
}
obj.foo() // obj对象


// 3.调用方式三: 通过call/apply调用
foo.call("abc"); // String {"abc"}对象
```



```javascript
var obj1 = {
  name: "why",
  foo: foo()    // 这里是直接调用 foo() 函数,并将其返回值赋给 foo 属性
}
```

实际上obj1是

```javascript
var obj1 = {
  name: "why",
  foo: window
}

obj1.foo()  // 报错
```



### this绑定规则

#### 默认绑定

- 独立的函数调用我们可以理解成函数没有被绑定到某个对象上进行调用



#### 隐式绑定

1. **通过对象调用函数**

```javascript
function foo() {
  console.log(this); // obj对象
}

var obj = {
  name: "why",
  foo: foo
}

obj.foo();
```

2. **案例一的变化**
   - 我们通过obj2又引用了obj1对象，再通过obj1对象调用foo函数；
   - 那么foo调用的位置上其实还是obj1被绑定了this；

```javascript
function foo() {
  console.log(this); // obj1对象
}

var obj1 = {
  name: "obj1",
  foo: foo
}

var obj2 = {
  name: "obj2",
  obj1: obj1
}

obj2.obj1.foo();
```

3. **隐式丢失**
   - 结果最终是window，为什么是window呢？
   - 因为foo最终被调用的位置是bar，而bar在进行调用时没有绑定任何的对象，也就没有形成隐式绑定；
   - 相当于是一种默认绑定；

```javascript
function foo() {
  console.log(this); // window对象
}

var obj1 = {
  name: "obj1",
  foo: foo
}

// 将obj1的foo赋值给bar
var bar = obj1.foo;
bar();
```

4. **显示绑定**

   隐式绑定有一个前提条件：

   - 必须在调用的`对象内部`有一个对函数的引用（比如一个属性）；
   - 如果没有这样的引用，在进行调用时，会报找不到该函数的错误；
   - 正是通过这个引用，间接的将this绑定到了这个对象上；

   如果我们不希望在 **对象内部** 包含这个函数的引用，同时又希望在这个对象上进行强制调用，该怎么做呢？

   - JavaScript所有的函数都可以使用call和apply方法（这个和Prototype有关）。

   - - 其实非常简单，第一个参数是相同的，后面的参数，apply为数组，call为参数列表；

   - 这两个函数的第一个参数都要求是一个对象，这个对象的作用是什么呢？就是给this准备的。

   - 在调用这个函数时，会将this绑定到这个传入的对象上。

   因为上面的过程，我们明确的绑定了this指向的对象，所以称之为 **显示绑定**。

   

#### call、apply

**通过call或者apply绑定this对象**

- 显示绑定后，this就会明确的指向绑定的对象

```javascript
function foo() {
  console.log(this);
}

foo.call(window); // window
foo.call({name: "why"}); // {name: "why"}
foo.call(123); // Number对象,存放时123
```

####  bind函数

**如果我们希望一个函数总是显示的绑定到一个对象上，可以怎么做呢？**

方案一：自己手写一个辅助函数（了解）

- 我们手动写了一个bind的辅助函数
- 这个辅助函数的目的是在执行foo时，总是让它的this绑定到obj对象上

```javascript
function foo() {
  console.log(this);
}

var obj = {
  name: "why"
}

function bind(func, obj) {
  return function() {
    return func.apply(obj, arguments);
  }
}

var bar = bind(foo, obj);

bar(); // obj对象
bar(); // obj对象
bar(); // obj对象
```

方案二：使用Function.prototype.bind

```javascript
function foo() {
  console.log(this);
}

var obj = {
  name: "why"
}

var bar = foo.bind(obj);

bar(); // obj对象
bar(); // obj对象
bar(); // obj对象
```













## 默认参数

```js
function show(name = "li", age = 18) {
    console.log(name, age);
}
```





## 生成随机数

```js
function rand(m: number, n: number) {
    return Math.ceil(Math.random() * (n - m + 1)) + m - 1;
}
```

## 数值

### parseFloat(~)

当你使用 `parseFloat()` 函数解析字符串时，它会尝试将字符串转换为浮点数。函数会从字符串的开头开始解析，直到遇到第一个无法转换为数字的字符为止。

```javascript
parseFloat("10g")  // 10
Number("10g")   // NaN
```



## Math

### 最小值 Math.min(~)

```js
let min = Math.min(-1, -2, -3);
```

### 最大值 Math.max(~)

```js
let max = Math.max(-1, -2, -3);
```

### 绝对值 Math.abs(x)

```js
console.log(Math.abs(-5)); // 输出: 5
console.log(Math.abs(3.14)); // 输出: 3.14
```

###  向上取整 Math.ceil(x)

```js
console.log(Math.ceil(4.2)); // 输出: 5
console.log(Math.ceil(-3.8)); // 输出: -3
```

### 向下取整 Math.floor(x)

```js
console.log(Math.floor(4.7)); // 输出: 4
console.log(Math.floor(-3.2)); // 输出: -4
```

### 四舍五入到最接近的整数 Math.round(X)

```js
console.log(Math.round(4.4)); // 输出: 4
console.log(Math.round(4.5)); // 输出: 5
```

### 介于0(包含)和1(不包含)之间的随机数 Math.random()

```js
console.log(Math.random()); // 输出: 0.123456789
console.log(Math.random()); // 输出: 0.987654321
```

### x的y次幂 Math.pow(x, y)

```js
console.log(Math.pow(2, 3)); // 输出: 8
console.log(Math.pow(4, 0.5)); // 输出: 2
```

### 平方根 Math.sqrt(x)

```js
console.log(Math.sqrt(16)); // 输出: 4
console.log(Math.sqrt(2)); // 输出: 1.4142135623730951
```

### 圆周率 Math.PI

```js
console.log(Math.PI); // 输出: 3.141592653589793
```



## Other

### var ctx = "\/jf_view\/"

```js
var ctx = "\/jf_view\/"    
//字符串中的 \/ 是一种转义序列，用于表示字符 /。在 JavaScript 中，/ 有特殊的含义，它用于表示正则表达式的开始和结束。如果要在字符串中包含 / 字符本身，需要使用 \/ 来转义它，以避免被解释为正则表达式的一部分。
```

### 0.1+0.2

在js中，0.1 + 0.2 不等于 0.3 的原因是由于浮点数的表示和计算精度问题。浮点数在计算机中的存储和运算遵IEEE 754标准，这种标准使用二进制格式来存储浮点数，包括符号位、指数和尾数三部分。由于二进制表示法对于某些十进制小数的不精确性，特别是像0.1和0.2这样的数在二进制中是无限循环小数，计算机只能存储它们的近似值。因此，当这两个近似值相加时，结果并不是精确的0.3，而是一个近似的值，如0.30000000000000004。



# TypeScript

## 认识TypeScript

- js无法在代码编译期间发现错误

错误出现的越早越好，能在写代码的时候发现错误，就不要在代码编译时发现错误。（ide的优势就是在代码编写过程中帮助我们发现错误）

- 为了弥补JavaScript类型约束上的缺陷，增加类型约束

2014年，facebook推出了flow对js进行类型检查； vue2/react => flow

同年，Microsoft微软也推出了Typescript1.0；现在ts已经完全胜出。

- TypeScript是拥有类型的JavaScript超集

不仅仅增加了类型约束，而且包括一些语法的扩展，比如枚举类型(Enum)、元祖类型(Tuple)等;

## TypeScript转js

- bable

 Babel是一个 JavaScript 编译器(转换引擎),将高版本语法转换成低版本语法。

一般使用bable 把ts转js

- tsc

```node.js
tsc xxx.ts   // 把ts文件编译为js文件
```



## 类型推导

let进行类型推导，推导出通用类型

const进行类型推导，推导出来为字面量类型



`typeof` 操作符返回的类型信息是基于 JavaScript 的运行时类型，而不是 TypeScript 的静态类型

```typescript
const str = 'hello wolrd';
console.log(typeof str); // string
const arr = ['a', 'b', 'c']
console.log(typeof arr); // object
```

1. 对于 `str`，它是一个字符串字面量，在 JavaScript 中，字符串的运行时类型就是 `'string'`。
2. 对于 `arr`，它是一个数组，在 JavaScript 中，数组是一种特殊的对象，因此 `typeof arr` 返回 `'object'`。

```typescript
const str = 'hello world';
type StrType = typeof str; // StrType 的类型是 'hello world'

const arr = ['a', 'b', 'c'];
type ArrType = typeof arr; // ArrType 的类型是 ['a', 'b', 'c']
```

### 类型校验

```ts
// 参数为一个对象，含有length属性且为数值类型
function getLength(args:{length : number}){
    return args.length;
}


const info = {
   	length:100
}

getLength(info);
getLength("hello")
getLength(['a','b','c'])
```





## 类型

### 数组

有两种写法

```typescript
let names: string[] = ["aaa","bbb"]
let nums: Array<number> = [123,111]
```



### 对象

```typescript
type InfoType={
    name: string
    age: number
}
let info: InfoType = {
    name："why",
    age: 18
}
// 或者
interface InfoType{
  name: string;
  age: number;
}

let info: InfoType = {
  name: 'Tom',
  age: 20
}
```

object对象类型可以用于描述一个对象，但是从myinfo中不能获取和设置数据

```typescript
const myInfo:object = {
    name:"li",
    age: 16
}

myInfo["name"]= "zhang"  // 元素隐式具有 "any" 类型，因为类型为 ""name"" 的表达式不能用于索引类型 "{}"。类型“{}”上不存在属性“name”。
console.log(muInfo.age)  // 类型“object”上不存在属性“age”。
```



### Symbol类型(js)

可以通过symbol类型定义相同的名称，因为symbol函数返回的是不同的值

```javascript
const person = {
    name : "li",
    name : "zhang"  //报错
}

// symbol
const s1: symbol = Symbol("name")
const s2: symbol = Symbol("name")

const person = {
    [s1]: "li",
    [s2]: "zhang"
}
```



### null & undefined

在typescript里，它们各自的类型也是undefined 和 null

```typescript
let n: null = null
let u: undefined = undefined
```



```javascript
console.log(typeof null === 'null') // false
console.log(typeof null === 'object')  // true
console.log( typeof undefined === 'undefined')  // true 
```

虽然 `typeof null` 返回 `"object"`,但这实际上是 JavaScript 的一个历史遗留 bug。`null` 本身是一个独立的类型



```javascript
0bject.prototype.toString.call(null)
```





### unKnown类型

用于描述类型不确定的变量

和any类型有点类似，但是unKnown类型的值上做任何事情都是不合法的

unKnown类型在默认情况下进行任何操作都是非法的

必须进行类型的校验（缩小）

```typescript
let foo: unknown = 'aaa';
foo = 123

console.log(foo.length) //非法

if(typeof foo === "string"){ // 类型缩小
    console.log(foo.length)
}
```



### void类型

如果一个函数返回值是void类型，也可以返回undefined

当基于上下文的类型推导推导出返回类型为void的时候，并不会强制函数一定不能返回内容。



### never类型

实际开发时很少会用到，一般是推导出为never

封装框架，工具库时会用到

**never表示永远不会发生值得类型，比如一个函数**

- 如果一个函数中是一个死循环或者抛出一个异常，这个函数不会返回东西，写void或者其他类型作为返回值都不合适，可以使用never类型。

1. 表示永不返回的函数

```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```



封装框架

```typescript
function handleMessage(message: string | number){
    switch(typeof message){
        case "string":
            console.log(message.length);
            break;
        case "number":
            console.log(message);
            break;
        default:
            const check: never = message;
    }
}
```

如果不加never，这种情况下message添加boolean类型，不会报错。

`never` 类型表示不应该有任何值能赋给它。



### tuple 元组类型

```typescript
const tInfo: [string,number,number] = ["why",18,1.88];
const item1 = tInfo[0];
const item2 = tInfo[1];

const info:(string | number)[] = ["why",18,1.88];  // 数组表示
```

一般使用对象类型保存

```typescript
cosnt info2 = {
    name:"why",
    age:18,
    height:1.88
}
```



### enmu 枚举类型

#### 数字类型枚举

如果我们枚举里面的内容不指定默认的值那么将会默认赋值 从0开始

```ty
enum NumerEnum {
    Zero,
    One,
    Two,
}

console.log(NumerEnum.Zero)  // 0
console.log(NumerEnum.One)  // 1
console.log(NumerEnum.Two)  // 2
```



如果其中一个被初始化，接下来的都会+1

```typescript
enum Direction {
  Up,   // 0
  Down = 3,  // 3
  Left,      // 4
  Right,     // 5
}
```



在函数中使用元组类型最多

```typescript
function useState<T>(init: T): [T, (newValue: T) => void] {
  let stateValue = init;
  function setValue(newValue: T) {
    stateValue = newValue;
    console.log("State updated to:", stateValue);
  }
  return [stateValue, setValue];
}

const [counter, setCounter] = useState(10);

console.log("Initial counter:", counter); // 输出: Initial counter: 10

setCounter(20); // 更新状态并输出: State updated to: 20
```





#### 字符串枚举

字符串的枚举是没有自增长的功能的

```typescript
enum Direction {
  Up = 'up',
  Down = 'down',
  Left = 'left',
  Right = 'right',
}
```



#### 使用枚举

```typescript
let playerDirection: Direction = Direction.Up;
console.log(playerDirection); // 输出：1

//枚举成员可以直接通过枚举类型来访问，也可以通过枚举的值来访问。
```







### 函数类型

```typescript
const foo: ()=>void = () => {
    console.log("hello")
}

type fooType = () => void;
const foo: fooType = () =>{
    xxx
}
```

```typescript
type CalcType = (num1: number, num2: number) => number;

const a1: CalcType = (num1, num2) => {
    return num1 + num2;
};
```



```typescript
function dealyFn(fn:()=>void){
	setTimeout(()=>{
        fn()
    },1000);
}
```



```typescript
type calcfunc =(num1: number, num2: number) => void
// 这里的num1，num2无法省略
```



### 联合类型 |

从现有类型中构建新类型

```ts
function getLength(args: string | any[]){
    return args.length;
}
```



### 交叉类型 &

表示一个值必须同时具备所有类型的特性

可以用于interface和type，但是只能定义type

```typescript
interface Person {
  name: string;
}

interface Employee {
  employeeId: number;
}

type EmployeePerson = Person & Employee;

const employee: EmployeePerson = {
  name: "Alice",
  employeeId: 123
};
```



```typescript
type MyType = number & string // 不存在，是never类型
```



### 类型别名

```typescript
type MyNumber = number
const age: MyNumber

type IDtype = number | string
function printID(id: IDType){
    console.log(id)
}
```



```typescript
type PointType = {x:number, y:number, z?:number }
function printCoordinate(point: PointType){
     xxxx
}

// 加逗号，分号，不加都可以
// 写在同一行需要加逗号或者分号
```



### 接口的声明

```typescript
interface PointType {
    x:number
    y:number
    z?:number
}
```



### interface & type

#### 区别

1. type 可以描述任何类型组合，interface 只能描述对象结构
2. interface 可以继承自（extends）interface 或对象结构的 type。type 也可以通过 `&` 做对象结构的继承；

```typescript
// Interface 继承 Interface
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

const myDog: Dog = {
  name: "Buddy",
  breed: "Golden Retriever"
};

// Interface 继承 Type
type AnimalType = {
  name: string;
}

interface Cat extends AnimalType {
  color: string;
}

const myCat: Cat = {
  name: "Whiskers",
  color: "Gray"
};

// Type 使用交叉类型
type Animal = {
  name: string;
}

type Bird = Animal & {
  canFly: boolean;
}

const myBird: Bird = {
  name: "Tweety",
  canFly: true
};

```

3. 多次声明的同名 interface 会进行声明合并，type 则不允许多次声明；

```typescript
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = {
  name: "Alice",
  age: 30
};
```

4. interface可以被类实现

```typescript
class Person implements IPerson{
    xxx
}
```





### 类型断言 as

有时候ts无法获取具体的类型信息，需要使用类型断言（Type Assertions）

  ```typescript
  const imgEl = document.querySelector("img") // const imgEl:HTMLImageElement | null
  const imgEl1 = document.querySelector(".img") // const imgEl:Element | null
  
  const imgEl1 = document.querySelector(".img") as HTMLImageElement
  ```

断言只能断言为更具体的类型，或者 不太具体的类型（any / unknown）

bug：

```typescript
const name = ("coderwhy" as unknown) as number
```



`as const` 用于将对象的所有属性设置为只读，并推断其为字面量类型



### 非空类型断言 !

比较危险，确定一定有值得时候才能使用

```typescript
interface IPerson{
    name:string,
    friend?:{
        name:string
    }
}

console.log(info.friend?.name) // 访问

// 方案1 类型缩小
if(info.friend){
    info.frined.name = "kobe"
}

// 方案2 非空类型断言
info.friend!.name = "james" 
// 如果你错误地断言了一个实际上是 null 或 undefined 的值，JavaScript 在运行时会抛出错误。
```

赋值表达式左侧不能是可选属性访问



### 字面量类型

```typescript
type Direction = "left" | "right"
const d1:Directon = "left"
alert(typeof d1);  // string
```

字面量类型（如 `"left" | "right"`）在编译时用于类型检查，而在运行时，`typeof` 只考虑实际的数据类型。



```typescript
type MethodType = "get" | "post"
function request(url:string,method:MethodType){
    xxx
}

const info = {
    url:"xxx",
    method:"post"
}

request(info.url,info.method) // info.method报错
/*
	const info:{
		url: string;
		method: string;
	}
*/

// 解决方案1
request(info.url,info.method as "post") // 进行类型断言

// 解决方案2
const info:{url: string, method: "post"} = {
    xxx
}

// 解决方案3
const info = {
    url:"xxx",
    method:"post"
} as const
```

`as const` 用于将对象的所有属性设置为只读，并推断其为字面量类型



### 类型缩小

```typescript
if(typeof padding === "number"){  // 类型保护
    xxx
}
```

#### in运算符

检查对象是否有某个属性

```typescript
const obj = { a: 1, b: 2 };
console.log("a" in obj); // 输出: true
console.log("c" in obj); // 输出: false
```

迭代对象的属性 for in

```javascript
const obj = { a: 1, b: 2 };
for (const prop in obj) {
  console.log(prop); // 输出: "a", "b"
}
```

缩小类型范围

```typescript
interface ISwim {
	swim: () => void
}
interface IRun {
    run: () => void
}

function move(animal: ISwim | IRun){
    if("swim" in animal){
        animal.swim()
    }else if ("run" in animal){
        animal.run()
    }
}
```



#### instance of

判断是否为某个类的实例

```typescript
function printDate(date: string | Date){
    if(date instanceof Date){
        console.log(date)
    }
}
```

```javascript
function Person() {
  this.name = "wang";
}
let p1 = new Person();
console.log(p1 instanceof Person); // true
```



#### keyof

keyof 是 TypeScript 中的一个关键字，用于获取某个类型的所有公共属性名（public property names）组成的联合类型（union type）。它可以与索引类型（indexed types）一起使用，以确保动态访问属性时的类型安全。







## 函数类型参数个数

```typescript
type CalaType = (num1:number, num2:number) => number
function calc(calcFn:CalaType){
    xxx
}

calc(function(){   // 这里参数个数不进行校验，ts不报错
    return 123
})
```



ts对于很多类型是否报错，取决于它的内部规则

```typescript
inetrface IPerson {
	name:string,
    age:number
}

const info:IPerson {
	name:"why",
    age:18,
    height:1.88   // 报错
}

const p = {
	name:"why",
    age:18,
    height:1.88
}

const info:IPerson = p  // 不报错
```



在js中调用时有两个参数，但是函数本身只有一个参数

额外的实参会被忽略

为回调函数写函数类型不要写成可选

typescript对于传入的函数类型的多余的参数会被忽略掉

```typescript
type CalaType = (num1:number, num2:number) => number
function calc (calcFn:CalcType){
    calcFn(10,20)
}

calc(function(num1)){
	return 123     
})
```



## Call Signatures 调用签名

在js中，函数除了可以被调用，自己也可以有属性值

```typescript
type BarType = (num1:number) => number  // 这种写法不支持声明属性
```

函数的调用签名(从对象的角度看待函数)

```typescript
interface IBar {
    name:string,
    age:number,
    (num1:number): number
}

const bar: IBar = (num1:number):number => {
    return 123
}

bar.name = "aaa"
bar.age = 18
bar(123)
// 如果你去掉 (num1: number): number，那么 IBar 就只定义了 name 和 age 属性，不能再将 bar 作为函数调用。代码会报错，因为 bar 不再符合接口的要求。
```



## Construct Signatures 构造签名

js函数也可以用new操作符调用，当贝调用的时候，ts会认为这是一个构造函数，因为他们会产生一个新对象。

你可以写一个构造签名（Construct Signatures），方法是在调用签名签名加一个new关键词

```typescript
class Person {
    constructor(public name: string, public age: number) { }
}
interface PersonConstructor {
    new(name: string, age: number): Person;
}

function createPerson(ctor: PersonConstructor): Person {
    return new ctor("Alice", 25);
}

// 调用 createPerson 函数,传入 Person 类作为构造函数
const person = createPerson(Person);
```



## 可选参数

```typescript
function foo(x:number,y?:number){  // y 的类型为 number | undefined
    if(y!==undefined){
        console.log(y+10)
    }
}
```

1. 默认有值的情况下，参数的类型注解可以省略
2. 有默认值的参数可以接收一个undefined的值

```typescript
function foo(x:number,y = 100){
    console.log(y+10)
}

foo(10,undefined) // 不报错
```



## js剩余参数

剩余参数将不定数量的参数放到一个数组里

```javascript
function sum(...nums:number[]){
    let total = 0;
    for(const num of nums){
        total += num;
    }
    return total;
}

const result1 = sum(10,20,30)
```



## 函数的重载

在typescript中我们可以边写不同的重载签名

一般编写两个或以上的重载签名，再去编写一个通用的函数以及实现

开发中尽量使用联合类型实现

```typescript
function add(arg1:number, arg2:number): number
function add(arg1:string, arg2:string): string

function add(arg1:any, arg2:any): any{
    return arg1 + arg2
}


add(10,20)
add("abc","cba")
add(111,"aaa")  // 报错，没有匹配的重载
```



获取长度，对象方法实现

```typescript
function getLength(arg:{length:number}){
    return arg.length
}
```



## this

在没有对ts进行特殊配置的情况下，this是any类型 

指定this

（跳过）



## 类

### 类的定义

在早期JavaScript开发中（ES5），通过函数和原型链实现类和继承，从ES6开始，引入了class关键字

typescript可以对类的属性和方法进行静态类型检测



typescript成员属性必须进行声明

<span style="color:red">构造函数不需要返回值，默认返回当前示例</span>

```typescript
class Person{
    name: string
    age: number
    
    constructor(name:string, age:number){
        this.name = name
        this.age = age
    }
    
    say(){
        console.log(this.name, this.age)
    }
}

const p1 = new Person("why",10)

console.log(p1.name, p2.age)
```



### 类的继承

使用`extends`关键字, 子类使用`super`访问父类

```typescript
class Student extends Person{
    sno: number
    constructor(name:string, age:number, sno:number){
        super(name,age)
        this.sno = sno
    }
    
    studying(){
        console.log(this.name)
    }
    
    saying(){
        super.say()
        consoloe.log("hello")
    }
}
```



### 类的成员修饰符

在TypeScript中，类的属性和方法支持三种修饰符：public、private、protected 

- public 修饰的是在任何地方可见、公有的属性或方法，默认编写的属性就是public的； 
- private 修饰的是仅在同一类中可见、私有的属性或方法； 可以在类内部构造器，方法里使用。可以用getter/setter设置
- protected 修饰的是仅在类自身及子类中可见、受保护的属性或方法



### 只读属性 readonly

1. 类属性

```typescript
class Person{
    readonly name: string
    constructor(name : string){
        this.name = name
    }
}

const p = new Person("li")
p.name = "zhang"  // 只读属性无法写入
```

2. 接口

```typescript
interface User{
    readonly id: number,
    name:string
}

```



### getter/setter

私有属性：属性前面加 _ 

可以对属性取值赋值进行拦截

```typescript
class Person{
    private _name: string
    constructor(name: string){
        this._name = name
    }
    
    set name(newValue: string){
        this._name = newValue
    }
    get name(){
        return this._name
    }
}

```



### 参数属性

TypeScript 提供了特殊的语法，可以把一个构造函数参数转成一个同名同值的类属性

以通过在构造函数参数前添加一个可见性修饰符public private protected 或者 readonly 来创建参数属性，最后这些类 属性字段也会得到这些修饰符

```typescript
class Person{
    constructor(public name: string, private _age: number){}
    
    set age(newAge){
        this._age = newAge
    }
    get age(newAge){
        return this._age
    }
}
```



### 抽象类 abstract

继承是多态的前提。封装，继承，多态。

es5通过原型链实现继承，es6可以通过extends继承

调用接口时, 我们通常会让调用者传入父类，通过多态来实现更加灵活的调用方式

但是，父类本身可能并不需要对某些方法进行具体的实现，所以父类中定义的方法,我们可以定义为抽象方法

- 抽象方法，必须存在于抽象类中；

- 抽象类是使用abstract声明的类；



### 具有类型特性

类的作用

1. 可以创建类对应的实例对象
2. 类本身可以作为这个实例的类型
3. 类也可以当做一个有构造签名的函数

```typescript
class Person{
    constructor(public name: string,public age: number){}
}

function factory(ctor: new() => void){}
factory(Person)
```





## 鸭子类型

typescript对于类型检测的时候是用鸭子类型

*“一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子,那么这只鸟可以被称为鸭子”*

鸭子类型只关心属性和行为，不关心具体是不是对应的类型

```typescript
class Person{
    constructor(public name: string,public age: number){}
}

class Dog{
    constructor(public name: string,public age: number){}
}

function printPerson(p:Person){
    console.log(p.name,p.age)
}

printPerson(new Person("li",17))
printPerson({name:"zhang",age:18})  // 不报错
printPerson(new Dog("旺财",9))  // 不报错
cosnt person: Person = new Dog("a",3) // 不报错

printPerson({ name1: "zhang", age: 18 })  // 报错
```



## 索引签名

```typescript
interface Iconllection{
    [index: number]: string  // 索引签名
    length: number
}

const names: string[] = ["abc", "bb"]
console.log(names[0])
console.log(names[1])

const tuple: [string,string] = ["why", "18"]
console.log(tuple[0])
console.log(tuple[1])
```



```typescript
interface Iconllection {
    [index: string]: string  // 索引签名
    length: number  // 报错，因为length也是string类型，被索引签名包括，但是返回的是number类型
}  
```

在定义 `Iconllection` 接口时，使用了索引签名 `[index: string]: string`，这意味着所有通过字符串索引访问的属性都必须是字符串类型。然而，你在接口中定义了一个名为 `length` 的属性，其类型为 `number`，这与索引签名的类型不匹配，因此导致了类型错误



## 接口继承



## 英文

argument: 参数

parameter: 形参

## declare type

`declare` 关键字用于声明已经存在于外部环境中的变量、函数、类、模块等。它告诉 TypeScript 编译器这些实体的类型信息，但不生成实际的代码。

`type` 则是用于创建类型别名的关键字，它允许你为一个类型起一个新的名字。



## const modelData1 = () => ({ component: VNode2 })

`const modelData1 = () => ({ component: VNode2 })` 这种写法是一个箭头函数，它返回一个对象。

- 箭头函数 `() => ({ ... })` 用于返回一个对象字面量。
- 外层的括号 `()` 是必需的，因为它告诉 JavaScript 解释器这是一个对象字面量，而不是函数体的开始。
- 内层的花括号 `{}` 定义了对象的属性和值。

```tsx
//等同于
const modelData1 = () => {
  return {
    component: VNode2
  };
};
```

## ReturnType

用于获取函数类型的返回值类型

```tsx
interface User {
  id: number;
  username: string;
  email: string;
}

function getUsername(user: User): string {
  return user.username;
}

type GetUsernameReturnType = ReturnType<typeof getUsername>;
                                        
const username: GetUsernameReturnType = getUsername({ id: 1, username: 'john', email: 'john@example.com' });
console.log(username); // 输出：'john'                                        
```



## 回调函数

回调函数的确可以理解为将一个函数传递给另一个函数,让被调用的函数在适当的时候执行传入的函数,并将结果返回给外部



### 示例

```tsx
export function use1(fun: (getTable: () => string) => number) {
  const tableData = 'Hello, World!';

  const getT2 = () => {
    return tableData;
  };

  const result = fun(getT2);
  console.log('Result:', result);
}

// 使用示例
use1((getT1) => {
  const table = getT1();
  return table.length;
});

getTable只是一个占位符，表示fun期望接收一个返回字符串的函数。
实际上将getT2函数传递给了fun函数，getT2扮演了getTable的角色

两层回调
fun由`使用use1的`提供，给use1使用，返回table.length
getTable由use1提供，给`使用use1的`使用 返回一个string


/**
use1(function(getT1) {
  const table = getT1();
  return table.length;
});
*/

等于传入一个匿名函数，第一个参数是getT1，返回的是 getT1的返回值的length



function simpleUse1(callback: (data: string) => number) {
  const tableData = 'Hello, World!';

  const result = callback(tableData);
  console.log('Result:', result);
}

// 使用示例
simpleUse1((data) => {
  return data.length;
});
```



## 非空断言操作符 `!` 

它用于告诉 TypeScript 编译器，某个值不会是 `null` 或 `undefined`，即使 TypeScript 无法确定这一点。

```typescript
function getValue(value?: string) {
  if (typeof value === "undefined") {
    return;
  }
  
  console.log(value.length); // 错误：Object is possibly 'undefined'.
  console.log(value!.length); // 正确：使用非空断言操作符
}
```





# jquery

```js
<li>
   分组：<select name="tpl_group">
   			<option value="" style="color:red" selected>-所有-</option>
   			<option value="--" style="color:red">-未分组-</option>
   			#for(item : t_list)
   				<option value="#@toXmlStr(item.CF_GROUP)">#@toXmlStr(item.CF_REMARK)</option>
   			#end
        </select>
</li>

var tpl_group = $("select[name='tpl_group']").val();
```

