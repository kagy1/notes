# html

超文本标记语言（Hyper Text Markup Language）

## 元素分类

### 行内元素

#### **特点**

行内元素也称内联元素，其特点是不会占据一行，也不强迫其他标签在新的一行显示，一般<span style="color:red">不可以设置宽度、高度、对齐等属性</span>，靠文本内容大小和图像尺寸填充，常用于控制页面中文本的样式。

- 相邻行内元素在一行，一行可以显示多个。
- 高度、宽度的设置无效，只会被文字撑开。
- 默认宽度就是文本撑开的长度

#### 元素

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

#### 特点

在页面中以区域块的形式出现，其特点是每个块元素通常都会独占一行或多行，可以对其设置宽度、高度、对齐等属性，常用于网页布局和网页结构的搭建。

- 自身独占一行
- 高度、宽度、内外边距都可以自定义
- 宽度默认是父元素的100%

#### 元素

- `<div>` - 通用块级容器
- `<p>` - 段落
- `<h1>` ~ `<h6>` - 标题
- `<ol>` - html有序列表
- `<ul>` - 无序列表
- `<li>` - 列表项，放在`<ol>`或者`<ul>`中
- `<table>` - 表格
- `<form>` - 表单
- `<hr>` - 水平分割线
- `<pre>` - 预格式化文本块
- `<blockquote>` - 长引用
- `<address>` - 联系方式信息
- `<article>`, `<section>`, `<nav>`, `<aside>`, `<header>`, `<footer>` - HTML5 语义化元素



### 行内块元素

#### 特点

既可以像块元素那样设置宽高，又可以像行内元素那样不独占一行。

- 和相邻的行内元素（包含行内块）在一行上，它们直接会有空白缝隙
- 一行可以显示多个（行内元素特点）
- 默认宽度就是内容的宽度（行内元素特点）
- 高度、宽度、内外边距都可以自定义（块元素特点）

#### 元素

- `<img>` - 图片
- `<input>` - 输入框
- `<button>` - 按钮
- `<select>` - 下拉选择框
- `<textarea>` - 多行文本框

1. 和相邻行内块元素再一行显示，但是他们之间会有空白缝隙。一行上可显示多个。
2. 默认宽度就是它本身内容的宽度。
3. 高度，宽度，行高，内边距，外边距都可以自己控制。

4. 在布局上表现得像一个行内元素(如文本),但同时也能像块级元素一样设置宽度、高度等属性。



## 标签

### `<a>`

#### target属性

可以使用 `<a>` 标签的 `target` 属性来设置链接在当前页面还是别的页面打开。`target` 属性指定在何处打开链接文档。以下是几个常用的值：

1. `_self`: 在同一个页面中打开链接（默认值）。

   ```html
   <a href="https://www.example.com" target="_self">Link</a>
   ```

2. `_blank`: 在新的未命名的浏览器窗口或标签页中打开链接。

   ```html
   <a href="https://www.example.com" target="_blank">Link</a>
   ```

3. `_parent`: 在父框架集中打开链接。如果没有父框架集，此选项的行为方式与 `_self` 相同。

   ```html
   <a href="https://www.example.com" target="_parent">Link</a>
   ```

4. `_top`: 在整个浏览器窗口中打开链接。如果没有父框架集，此选项的行为方式与 `_self` 相同。

   ```html
   <a href="https://www.example.com" target="_top">Link</a>
   ```

5. `framename`: 在指定的命名的 iframe 中打开链接。

   ```html
   <iframe name="myframe"></iframe>
   <a href="https://www.example.com" target="myframe">Link</a>
   ```



### `<radio>`

```html
<input type="radio" name="group3" value="1"> Option 1
<input type="radio" name="group3" value="2"> Option 2
<input type="radio" name="group3" value="3"> Option 3
```



### `<select>`

```html
<select name="group1">
  <option value="1">Option 1</option>
  <option value="2">Option 2</option>
</select>
```



v-model绑定：

```vue
<select v-model="v1">
  <option value="1">Option 1</option>
  <option value="2">Option 2</option>
</select>
```



### `<fieldset>`,`<legend>`

`<fieldset>`标签是一个块级容器标签，表示控件的集合，用于将一组相关控件组合成一组

`<legend>`标签用来设置`<fieldset>`控件组的标题，通常是`<fieldset>`内部的第一个元素，会嵌入显示在控件组的上边框里面

```vue
<form>
  <fieldset>
    <p>年龄：<input type="text" name="age"></p>
    <p>性别：<input type="text" name="gender"></p>
  </fieldset>
</form>
```

上面代码中，两个输入框是一组，它们的外面会显示一个方框



## HTML5

### 新特性和改进

- 语义化标签:如`<header>`,`<nav>`,`<article>`,`<section>`,`<aside>`,`<footer>`等,使文档结构更清晰。
- 多媒体支持:如`<video>`和`<audio>`标签,无需插件即可播放音视频。
- 图形支持:如`<canvas>`和`<svg>`标签,可以绘制2D和3D图形。
- 离线存储:如localStorage和sessionStorage,可以在客户端存储数据。
- 网络支持:如WebSocket,可以实现全双工通信。
- 表单增强:如新的input类型(date、time、email等),表单验证等。
- 其他:如Geolocation、Web Worker、History API等。



# css

## 选择器

1. 类选择器（Class Selectors）：

   - .className { ... } 选择所有具有class="className"的元素
   - p.intro { ... } 选择所有具有class="intro"的 <p> 元素

2. 属性选择器（Attribute Selectors）：

   选择含有指定属性的元素

   - [attribute] { ... } 选择具有指定属性的元素
   - [attribute="value"] { ... } 选择具有指定属性和值的元素
   - [attribute~="value"] { ... } 选择属性值包含指定词汇的元素
   - [attribute^="value"] { ... } 选择属性值以指定值开头的元素
   - [attribute$="value"] { ... } 选择属性值以指定值结尾的元素
   - [attribute*="value"] { ... } 选择属性值包含指定值的元素

3. 后代选择器：

   - div p { ... } 选择div内部的所有p元素

4. 子元素选择器（Child Selectors）：

   - parent > child { ... } 选择parent元素的直接子元素child

5. 相邻兄弟选择器（Adjacent Sibling Selectors）：

   - element1 + element2 { ... } 选择紧跟在element1后面的element2元素

6. 通用兄弟选择器（General Sibling Selectors）：

   - element1 ~ element2 { ... } 选择element1之后的所有element2同级元素

7. 伪类选择器（Pseudo-classes）：

   - :hover { ... } 选择鼠标悬停的元素
   - :active { ... } 选择被激活的元素
   - :focus { ... } 选择获得焦点的元素
   - :first-child { ... } 选择父元素的第一个子元素
   - :last-child { ... } 选择父元素的最后一个子元素
   - :nth-child(n) { ... } 选择父元素的第n个子元素
   - :nth-last-child(n) { ... } 选择父元素的倒数第n个子元素
   - :first-of-type { ... } 选择父元素下第一个指定类型的子元素
   - :last-of-type { ... } 选择父元素下最后一个指定类型的子元素
   - :only-child { ... } 选择父元素下唯一的子元素
   - :only-of-type { ... } 选择父元素下唯一指定类型的子元素
   - :not(selector) { ... } 对selector取反
   - :root { ... } 选择文档的根元素，可以用来定义全局变量

### 其他示例

```css
.a1.a2 {  // 同时拥有a1和a2类
  color: red;
}

.a1 .a2 {  // 选择a1类内部的子元素，子元素含a2类
  color: blue;
}

.a1, .a2 {  // 同时选择a1，a2  
  color: red;
}
```







## 数值计算

```css
/* 使用加法 */
.element {
  width: calc(100% - 20px);
}
 
/* 使用减法 */
.element {
  padding: calc(10px - 2px);
}

/* 使用乘法 */
.element {
  font-size: calc(16px * 1.2);
}
 
/* 使用除法 */
.element {
  line-height: calc(24px / 1.5);
}
```

## 自定义属性

### 定义

1. 使用`--`作为前缀
2. 变量通常定义在一个选择器中，如 :root 或者如何其他选择器

### 使用

1. 使用var()函数来引用变量的值
2. var()函数接受变量名作为参数，并返回变量的值
3. 可以在任何接受属性的地方使用var()函数

```css
:root {
  --primary-color: #007bff;
  --font-size: 16px;
  --spacing: 10px;
}
 
.button {
  background-color: var(--primary-color);
  font-size: var(--font-size);
  padding: var(--spacing);
}
```



变量默认值

可以在var()函数中提供第二个参数作为变量的默认值

```css
.button {
  background-color: var(--button-color, #000); /* 如果 --button-color 未定义，则使用 #000 作为默认值 */
}
```



## css模块化

CSS 模块化是一种将样式作用域局限于特定组件的技术，避免样式在全局作用域下的冲突。其核心思想是：每个组件的样式文件只服务于该组件。

### 实现方式

1. **CSS Modules**：自动为类名添加哈希，避免冲突。

   命名规范：`xxx.module.scss`

   ```vue
   // u4.module.scss
   .d1 {color:red}
   
   import { defineComponent, onMounted } from 'vue';
   import styles from './u4.module.scss';
   
   export default defineComponent({
       setup() {
           return () => (
               <div class={styles.d1}>
                   hello world
               </div>
           );
       },
   });
   ```

   原理：

   打包工具会对CSS模块文件进行处理。它会分析模块中的类名,并对每个类名进行转换,生成一个唯一的哈希值。例如,如果`u4.module.scss`文件内容如下

   ```css
   .d1 {
     color: red;
   }
   ```

   打包完成后为

   ```css
   .u4-module__d1__a1b2c3 {
     color: red;
   }
   ```

   ```vue
   <div class="u4-module__d1__a1b2c3">
     hello world
   </div>
   ```

   

2. **Scoped CSS**：在 Vue 中通过 `<style scoped>` 实现样式隔离。（为标签添加data-v-哈希值 属性）

3. **CSS-in-JS**：用 JavaScript 管理样式（如 styled-components）。



## 样式穿透

样式穿透的写法有三种：`>>>`、`/deep/`、`::v-deep`

`>>>` ：如果项目使用的是css原生样式，那么可以直接使用 `>>>` 穿透修改

```css
div >>> .cla {
	color: red;
}
```

`/deep/`：

```css
/*  用法：  */
div /deep/ .cla {
	color: red;
}
```

`::v-deep`

```css
/*  用法：  */
div ::v-deep .cla {
	color: red;
}
```



原理

scoped后选择器最后默认会加上当前组件的一个标识，比如[data-v-49729759]
用了样式穿透后，在deep之后的选择器最后就不会加上标识。





## 属性

###  height: 100vh

 height: 100vh; /* 设置容器高度为视口高度 */

### width: 100%

这里相对的是包含块的宽度

### content

在 CSS 中，`content` 属性只能用于伪元素（如 `::before` 和 `::after`），而不能直接用于普通元素

它可以插入文本或其他内容到元素中。

#### 用法

- **文本插入**: 在伪元素中插入字符串。

  ```css
  .example::before {
      content: "Hello";
  }
  ```

- **图片插入**: 可以通过 URL 插入图像。

  ```css
  .example::after {
      content: url('image.png');
  }
  ```

- **计数器**: 与 CSS 计数器结合使用。

  ```css
  .example::before {
      content: counter(my-counter);
  }
  ```

#### 特点

- **只能用于伪元素**: `content` 属性对普通元素无效。

- **与 `display` 搭配**: 通常与 `display: block` 或 `inline-block` 等结合使用，使伪元素可见。

- **支持动态内容**: 可以通过 `attr()` 函数使用 HTML 属性值。

  ```css
  .example::before {
      content: attr(data-info);
  }
  ```

#### 例子

```html
<div class="example" data-info="Dynamic Content">Original Content</div>
```

```css
.example::before {
    content: "Prepended ";
}

.example::after {
    content: attr(data-info);
}
```

#### 注意事项

- **不会影响文档流**: 伪元素的内容不影响文档结构。
- **兼容性**: 大多数现代浏览器都支持 `content` 属性。





### position

1. static（默认值）:
   - 元素按照正常的文档流进行排列
   - top、bottom、left、right属性无效
2. relative:
   - 元素按照正常的文档流进行排列
   - 可以使用top、bottom、left、right属性相对于其正常位置进行偏移
   - 元素原本所占的空间仍然保留
3. absolute:
   - 元素脱离文档流
   - 可以使用top、bottom、left、right属性相对于最近的非static定位祖先元素进行偏移
   - 如果没有非static定位的祖先元素，则相对于初始包含块（通常是视口）进行偏移
   - 元素原本所占的空间不再保留
4. fixed:
   - 元素脱离文档流
   - 可以使用top、bottom、left、right属性相对于<span style="color:red">浏览器窗口</span>进行偏移
   - 元素原本所占的空间不再保留
   - <span style="color:red">即使页面滚动，元素仍然保持在固定位置</span>
5. sticky（CSS3新增）:
   - 元素在跨越特定阈值前为相对定位，之后为固定定位
   - 可以使用top、bottom、left、right属性定义偏移量
   - 常用于创建粘性头部或侧边栏，当页面滚动到一定位置时，元素会固定在屏幕上



### background

`background` 是一个复合属性，它可以包含多个背景相关的属性

1. `background-color`：设置背景颜色。
2. `background-image`：设置背景图像。
3. `background-repeat`：设置背景图像的重复方式。
4. `background-position`：设置背景图像的位置。
5. `background-size`：设置背景图像的大小。
6. `background-attachment`：设置背景图像是否随页面滚动。
7. `background-origin`：设置背景图像的定位区域。
8. `background-clip`：设置背景的绘制区域。

这些属性可以按照任意顺序组合在 `background` 属性中，用空格分隔

```css
.example {
  background: #ff0000 url("image.jpg") no-repeat center/cover fixed;
}
```



### overflow

可以用于任何块级或内联块级元素

1. **visible**: 默认值，内容不会被剪裁，会溢出元素框。
2. **hidden**: 内容会被剪裁，超出部分不可见。
3. **scroll**: 内容会被剪裁，但会出现滚动条（无论是否需要）。
4. **auto**: 当内容溢出时，会自动显示滚动条，否则不显示。
5. **clip**: 类似于 `hidden`，但是不允许滚动。



### white-space

1. `white-space: normal`（默认值）:
   - 连续的空白字符会被合并为一个
   - 换行符会被当作空白字符处理
   - 必要时会对文本进行换行，以适应容器宽度
2. `white-space: nowrap`:
   - 连续的空白字符会被合并为一个
   - 换行符会被当作空白字符处理
   - 文本不会自动换行，除非遇到 `<br>` 标签
3. `white-space: pre`:
   - 连续的空白字符会被保留
   - 换行符会被保留
   - 文本只在遇到换行符时才换行
4. `white-space: pre-wrap`:
   - 连续的空白字符会被保留
   - 换行符会被保留
   - 文本会在必要时自动换行，并在遇到换行符时换行
5. `white-space: pre-line`:
   - 连续的空白字符会被合并为一个
   - 换行符会被保留
   - 文本会在必要时自动换行，并在遇到换行符时换行
6. `white-space: break-spaces`（CSS3新增）:
   - 连续的空白字符会被保留
   - 换行符会被当作空白字符处理
   - 文本会在必要时自动换行，并在遇到换行符时换行
   - 任何保留的空白序列总是会占用空间，包括在行尾





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

#### transform-origin

设置变换中心

```css
transform-origin: center;
transform-origin: left top; /* 设置原点为元素的左上角 */
```



### transition

`transition` 是用来设置元素状态之间的平滑过渡效果的。它通常是在某个状态（例如 `hover`、`focus`）发生变化时，平滑地过渡到另一个状态。

#### 属性

```css
transition: [property] [duration] [timing-function] [delay];
```

- **`property`**：指定需要过渡的 CSS 属性（如 `width`、`background-color`、`transform` 等）。可以设置为 `all`，表示所有支持的属性。
  - all：所有可动画属性
  - none：无过渡效果
  - CSS 属性名：如 width, height, color, background-color 等
- **`duration`**：过渡持续时间（如 `1s`, `500ms`）。
- **`timing-function`**：过渡效果（如 `ease`, `linear`, `ease-in-out`）。
  - ease：默认值，慢速开始，然后加快，然后慢速结束
  - linear：匀速
  - ease-in：慢速开始
  - ease-out：慢速结束
  - ease-in-out：慢速开始和结束
  - cubic-bezier(n,n,n,n)：在 cubic-bezier 函数中自定义速度曲线，n 取值范围为 0 到 1
  - steps(n, start|end)：分步过渡效果，n 为正整数，指定过渡分几步完成，可选 start 或 end
- **`delay`**：过渡开始前的延迟时间（如 `0s`, `200ms`）。





### cursor

```css
.d1 {
    cursor: pointer;  // 添加鼠标样式，表示可以点击
}
```

## 函数

### clamp（）

设置响应式的值范围，在最小值和最大值直接动态变化

```css
h1 {
    width: clamp(100px, 30%, 200px);
}
```

语法降级

```css
h1 {
 	width: max(100px, min(30%, 200px));   
}
```







## 颜色渐变

1. 线性渐变 linear-gradient

使用 `background` 或 `background-image` 属性来设置

```css
.linear-gradient {
  width: 300px;
  height: 150px;
  background: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);
}
```

第一个参数为渐变方向

1. 角度值 (Angle):
   - 使用 "deg" 单位指定角度,如 "45deg"。
   - 0deg 表示从下到上,90deg 表示从左到右,180deg 表示从上到下,270deg 表示从右到左。
   - 也可以使用负值,如 "-45deg",表示逆时针旋转45度。
2. 方位关键字 (Directional Keywords):
   - "to top": 从下到上渐变。
   - "to right": 从左到右渐变。
   - "to bottom": 从上到下渐变。(默认值)
   - "to left": 从右到左渐变。
   - "to top right": 从左下角到右上角渐变。
   - "to bottom right": 从左上角到右下角渐变。
   - "to bottom left": 从右上角到左下角渐变。
   - "to top left": 从右下角到左上角渐变。

3. 径向渐变 (Radial Gradient)

```css
.radial-gradient {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, red, yellow, green);
}
```









## border

```css
border: 1px solid black;
```

这三项顺序可随意更改

分别代表 `border-width`、`border-style`、`border-color`

### border-style

- `solid`：实线

- `dotted`：点状边框
- `dashed`：虚线边框
- `double`：双实线边框
- `groove`：凹槽边框
- `ridge`：脊状边框
- `inset`：内嵌边框
- `outset`：外嵌边框
- `none`：无边框
- `hidden`：隐藏边框





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



## table

### border-collapse

- 用于决定表格单元格的边框是否合并为一个单一的边框。
- 可能的值包括：
  - `separate`（默认值）：单元格之间的边框独立，不合并。
  - `collapse`：相邻单元格的边框合并为一个单一的边框。

```css
border-collapse: collapse / separate;
```

### border-spacing

- 用于设置相邻单元格边框之间的间距。
- 接受一个或两个长度值：
  - 如果提供一个值，表示水平和垂直方向的间距相同。
  - 如果提供两个值，第一个值表示水平间距，第二个值表示垂直间距。
- 默认值为 `0`，即单元格之间没有间距。
- 仅在 `border-collapse` 为 `separate` 时生效。





## BFC

**Block format context ，块级格式上下文**

一块独立渲染区域，触发bfc的元素会形成一个独立的渲染区域，这个区域里面的元素（包括他自己）不会影响外部元素的渲染，

什么情况下，会形成bfc区域呢？

1. 当添加了float属性，且float不为none时

2. 块元素添加overflow属性，且属性不能为visible（hidden、auto、scroll）

3. 类型强制转换：display：inline-block、、table-cell、table-caption；

4. 添加定位，positio值为absolute或fixed

5. 有根元素、父元素或其他包含元素

### 作用

1. 避免外边距重叠
2. 容纳浮动元素

```html
<div style="border: 2px solid red;overflow: hidden;">
    <div style="width: 100px;height: 100px;float: left;">
    </div>
</div>
```

3. 阻止文字环绕

4. 自适应两栏布局

 



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

求AAA和BBB之间的距离

```html
<style>
    p {
        font-size: 16px;
        line-height: 1;
        margin-top: 10px;
        margin-bottom: 15px;
    }
</style>

<p>AAA</p>
<p></p>
<p></p>
<p></p>
<p>BBB</p>

// 答案15px
```

相邻元素的`margin-top`和`margin-bottom`会发生重叠

空白的`<p>`也会重叠



#### margin负值

- `margin-top`,`margin-left`设置负值，元素向上、向左移动

- `margin-bottom `负值，下方元素上移，自身不受影响
- `maring-right`负值，右方元素左移，自身不受影响



#### margin: auto



### box-sizing

以特定的方式定义匹配某个区域的特定元素

默认值: content-box

#### content-box（默认）

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

<img src="D:/notes/前端basic/img/2.jpg" style="width:40%" align="left">

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

<img src="D:/notes/前端basic/img/3.jpg" style="width:40%" align="left">



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



## 伪元素

**定义：**伪元素用于创建一些不在文档(DOM)树中的元素，并为其添加样式。比如说，我们可以通过:before来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

伪元素使用2个冒号，常见的有：`::before`，`::after`，`::first-line`，`::first-letter`，`::selection`、`::placeholder`等；







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

###  旋转正方形

```html
<!DOCTYPE html>
<html lang="en" height="100%">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>旋转正方形</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
        }

        .container {
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }

        .square {
            position: absolute;
            height: 200px;
            width: 200px;
            background-color: yellow;
            opacity: 0.7;
            transform-origin: center;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="square"></div>
        <div class="square"></div>
        <div class="square"></div>
        <div class="square"></div>
        <div class="square"></div>
    </div>
</body>
<script>
    const squares = document.querySelectorAll('.square');
    const totalSquares = squares.length;
    const rotationAngle = 360 / totalSquares;

    squares.forEach((square, index) => {
        square.style.transform = `rotate(${index * rotationAngle}deg)`;
    });
</script>

</html>
```



### 两边固定，中间自适应

#### float实现

```html
<!DOCTYPE html>
<html lang="en" style="height: 100%;">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .left,
        .right {
            height: 400px;
            background-color: hotpink;
        }

        .left {
            float: left;
            width: 200px;

        }

        .right {
            float: right;
            width: 100px;
        }

        .center {
            height: 400px;
            background-color: skyblue;
        }
    </style>
</head>

<body>
    <div class="left"></div>
    <div class="right"></div>
    <div class="center"></div>
</body>

</html>
```

1. `.left` 元素首先被声明，并设置为左浮动（`float: left`）。它脱离了文档流，其他元素将环绕在它的右侧。
2. `.right` 元素接下来被声明，并设置为右浮动（`float: right`）。它也脱离了文档流，其他元素将环绕在它的左侧。
3. `.center` 元素最后被声明，没有设置浮动。由于前面的两个元素已经脱离了文档流，`.center` 元素会自动填充剩余的空间，位于两个浮动元素之间。



### 双飞翼布局

1. 两侧内容宽度固定，中间内容宽度自适应
2. 三栏布局，中间一栏最先加载、渲染出来







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


