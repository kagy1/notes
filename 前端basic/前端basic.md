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



# css

## flex布局

### flex:1

等同于: flex: 1 1 0

**flex: 1** 实际上是三个属性的缩写：**flex-grow: 1; flex-shrink: 1 flex-basis: auto;**

## font

### font-weight (字体粗细设置)

font-weight: normal / bold / bolder / lighter / 数字值(从100到900的整数) 

# js

## dom

### 事件

#### input事件和change事件区别

- `input`事件在每次值发生变化时都会立即触发，因此它提供了实时的反馈。当你需要实时响应用户的输入并执行相应的操作时，可以使用 `input` 事件。
- `change` 事件只在值发生变化并失去焦点后触发，因此它提供了延迟的反馈。当你需要在用户完成输入后再执行相应的操作时，可以使用 `change` 事件。



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

```js
const arr = [1,2,3,4]
const arr2 = [...arr]
// arr2 [1,2,3,4]
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

```js
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

```js
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
```

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



## Other

1. var ctx = "\/jf_view\/"

```js
var ctx = "\/jf_view\/"    
//字符串中的 \/ 是一种转义序列，用于表示字符 /。在 JavaScript 中，/ 有特殊的含义，它用于表示正则表达式的开始和结束。如果要在字符串中包含 / 字符本身，需要使用 \/ 来转义它，以避免被解释为正则表达式的一部分。
```



# TypeScript

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

