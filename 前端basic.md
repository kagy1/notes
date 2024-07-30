# html

## 元素分类

- 行内元素：

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

- 块元素:

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

- 行内块元素：

  既可以像块元素那样设置宽高，又可以像行内元素那样不独占一行。

  - `<img>` - 图片
  - `<input>` - 输入框
  - `<button>` - 按钮
  - `<select>` - 下拉选择框
  - `<textarea>` - 多行文本框



# css

## flex布局

### flex:1

等同于: flex: 1 1 0

**flex: 1** 实际上是三个属性的缩写：**flex-grow: 1; flex-shrink: 1 flex-basis: auto;**

## font-weight

font-weight:bold //加粗



# js

## dom

### input事件和change事件区别
- `input`事件在每次值发生变化时都会立即触发，因此它提供了实时的反馈。当你需要实时响应用户的输入并执行相应的操作时，可以使用 `input` 事件。
- `change` 事件只在值发生变化并失去焦点后触发，因此它提供了延迟的反馈。当你需要在用户完成输入后再执行相应的操作时，可以使用 `change` 事件。

### 



## es6

### proxy 代理对象

**拦截**

//以前使用数据劫持  obj.defineProperty()

```ts
let proxyObj = new Proxy(target, handle);
```



## for

```tsx
for…of输出的是数组的值，for…in输出的数组的索引，并且输出的索引是字符串
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







## 数组方法

arr.push   向数组末尾添加

```js
const arr = [];
arr.push(3);
arr.push(2);
arr.push(1);
// [3, 2, 1]
```

arr.unshift  将元素插入到数组的起始位置，并将其他元素向后移动

```js
const arr = [];
arr.unshift(3);
arr.unshift(2);
arr.unshift(1);
// [1,2,3]
```



## 生成随机数

```js
function rand(m: number, n: number) {
    return Math.ceil(Math.random() * (n - m + 1)) + m - 1;
}
```



## Math

### 最小值 Math.min(~)

```js
min = Math.min(min, prices[i]);
```

### 最大值 Math.max(~)

```js
max=Math.max(max,prices[i]);
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



# ts

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

## 
