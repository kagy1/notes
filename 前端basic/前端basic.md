# js

## 数据类型

### Number

#### 特殊数值

Infinity：无穷大

```javascript
var a = 1 / 0;  // Infinity
```

NaN： not a number

```javascript
var b = 1 * 'a';  // NaN
```



### 进制

```javascript
var num = 0x100;  // 十六进制
var num2 = 0o100;  // 八进制
var num5 = 0b100;  // 二进制
console.log(num, num2, num5)  // 256 64 4
```



### 数字范围

 最小正数值：Number.MIN_VALUE

 最大正数值：Number.MAX_VALUE    

 最大整数： Number.MAX_SAFE_INTEGER



## String

转义字符：

`\'`：单引号

`\"`：双引号

`\\`：反斜杠

`\n`：换行符

`\r`：回车符

`\t`：制表符

`\b`：退格符



## undefined

声明一个变量，没有对其初始化，它默认就是undefined



## Object

```typescript
var c1 = null;
console.log(typeof c1);  // object
// 这是 JavaScript 语言的一个历史遗留问题
```



## 类型转换

Number类型显式转换：Number(~)

Number()、Boolean()、String()

```javascript
Number(undefined) // NaN
Number(true)  // 1
Number(false) // 0
Number(NaN) // NaN
Number("aa") // NaN
```



parseInt()：

- 解析字符串并返回指定基数的整数

```javascript
parseInt("100", 2)  // 4
```

- 字符串"100"表示一个二进制数。
- 基数为2,表示将字符串解析为二进制数



parseFloat()：解析字符串并返回浮点数

- 解析字符串并返回指定基数的整数
- 如果遇到第一个非数字字符(除了小数点),解析停止,返回已解析的数字部分

```javascript
parseFloat("3.14abc");   // 返回 3.14
```



toString()：将值转换为字符串类型

```javascript
(42).toString()
```

toFixed()：将数字转换为指定小数位数的字符串

```javascript
(3.14159).toFixed(2)  // 3.14
```

valueOf()：返回对象的原始值。对于许多内置对象,valueOf()方法返回对象的基本类型值

```javascript
const num = new Number(42);
console.log(num.valueOf());  // 输出 42

const str = new String("hello");
console.log(str.valueOf());  // 输出 "hello"

const bool = new Boolean(true);
console.log(bool.valueOf()); // 输出 true
```

Array.from()：将类数组对象或可迭代对象转换为数组

```javascript
const str = "hello";
const arr1 = Array.from(str);
console.log(arr1);  // 输出 ["h", "e", "l", "l", "o"]

const obj = { length: 3, 0: "a", 1: "b", 2: "c" };
const arr2 = Array.from(obj);
console.log(arr2);  // 输出 ["a", "b", "c"]
```

JSON.stringify()：将JavaScript对象转换为JSON字符串

```javascript
const obj = { name: "John", age: 30, city: "New York" };
const jsonStr = JSON.stringify(obj);
console.log(jsonStr);  // 输出 '{"name":"John","age":30,"city":"New York"}'
```

JSON.parse()：将JSON字符串转换为JavaScript对象

```javascript
const jsonStr = '{"name":"John","age":30,"city":"New York"}';
const obj = JSON.parse(jsonStr);
console.log(obj);  // 输出 { name: "John", age: 30, city: "New York" }
```



## 运算符

`**` 幂（ES7）

### 赋值运算符

```javascript
var num1 = num2 = num3  // 链式赋值，从右到左进行计算
```

### 自增&自减

```javascript
var cur = 100
var cur1 = 100
var res = cur++  // 100
var res1 = ++ cur1  // 101 
```



## foo、bar、baz

伪变量

通常作为函数、变量、文件的名词，本身没有特别的用途和意义



## 函数

### arguments

在函数里都存在一个变量叫arguments，还有this

arguments是一个对象，对象内部包含了所有传入的参数

<span style="color:red">只有普通函数有，箭头函数没有</span>

```javascript
function print(name, age) {
    console.log(arguments);
    console.log(arguments[0])
}

print("li", 14)
```



### 函数表达式

函数表达式是在代码执行到达时被创建，仅从那一刻起可用

```javascript
var bar = function() {
    console.log("hello");
};
```



### 高阶函数

满足一下条件之一：

- 接受一个或多个函数输入
- 输出一个函数



### IIFE

**Immediately Invoked Function Expression"（立即执行函数表达式）**

```javascript
(function(){
    console.log("hello")
})()

(function(name) {
    console.log(name)  // li
})("li")

var res = (function(name) {
    return name
})("li")
```



**会创建一个独立的执行上下文环境，可以避免外界访问或修改内部的变量，也避免的对内部变量的修改**

```javascript
var res = (function() {
    var xmMoudle = {}
    var message = "hello XM"
    xmModule.message = message
    return xmMoudle  // 把需要的return出去
})()
```

点击打印

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

</head>

<body>
    <button class="btn">1</button>
    <button class="btn">2</button>
    <button class="btn">3</button>
    <button class="btn">4</button>
</body>
<script>
    var btns = document.querySelectorAll('.btn');
    for (var i = 0; i < btns.length; i++) {
        var btn = btns[i];
        (function (m) {
            btn.onclick = function () {
                console.log(m + 1);
            }
        })(i)
    }
</script>

</html>
```







## 作用域

### 函数作用域

在es5以前，没有块级作用域的概念，但是函数可以定义自己的作用域

函数作用域表示在函数内部定义的变量，只有在函数内部可以被访问到

**全局变量**：全局作用域

```javascript
var name = 'li'
```

var 定义的变量没有块级作用域

```javascript
{
    var count = 100
}
console.log(count) // 100
```

for循环也没有自己的作用域



**ES5之前函数代码块会形成自己的作用域**

函数内部定义的变量只有在函数内部能访问到

```javascript
function test() {
    var bar = "bar"
}
test()
console.log(bar);  // 报错
```

<span style="color:red">通过var声明的变量会在window对象上添加一个属性</span>



## 对象

```javascript
var Person = {
    name: "li",
    age: 18
}

console.log(Person["name"]) // li
console.log(Person.name) // li
```

对象不是可迭代对象，无法使用for of遍历

### 删除属性

delete

```javascript
var Person = {
    name: "li",
    age: 18
}
delete Person.age
```



### 计算属性名

它允许你使用表达式作为属性名，表达式的结果将作为实际的属性名

```javascript
const key = 'name';
const obj = {
  [key]: 'John',
  ['age']: 30,
  [1 + 2]: 'three'
};
console.log(obj); // { name: 'John', age: 30, '3': 'three' }
```

```javascript
const obj = {
    name: 'li'
}
let n1 = 'name'
console.log(obj[n1]) // li
```





### 创建对象

#### 工厂函数

```javascript
function createStudent (name, age, height) {
    var stu = {}
    stu.name = name
    stu.age = age
    stu.height = height
    stu.running = function() {
        console.log("is running")
    }
    return stu
}

var stu1 = createStudent("why", 18, 1.88) // 创建对象
```



#### 构造函数

在ES5之前都是通过`function`来声明一个构造函数（类），之后通过new关键字对其调用

ES6之后，JavaScript可以通过class声明类

*构造函数使用大驼峰，普通函数使用小驼峰*

```javascript
function CreateStudent (name, age, height) {
    this.name = name
    this.age = age
    this.height = height
    this.running = function() {
        console.log("is running")
    }
}

var stu1 = new CreateStudent("why", 18, 1.88)
```



#### new操作符调用

如果一个函数被new操作符调用了，那么它会执行如下操作：

1. 在内存里创建一个新的对象（空对象）
2. 这个对象内部的 [[prototype]] 属性会被赋值为该构造函数的`prototype`属性
3. 构造函数内部的this，会指向创建出来的新对象
4. 执行函数的内部代码（函数体代码）
5. 如果构造函数没有返回空对象，则返回创建出来的新对象



## 全局对象window

查找变量时，最终会查找到window上

使用var定义的变量会被默认添加到window上面

alert()方法就是window提供的

1. alert()：显示一个警告框，带有一条指定的消息和一个确认按钮。

2. confirm()：显示一个对话框，带有一条指定的消息和确认、取消按钮。用户可以选择确认或取消。

   confirm()方法会返回一个布尔值：如果用户点击确认，返回true；如果用户点击取消，返回false。

3. prompt()：显示一个对话框，提示用户输入一些文本。它返回用户输入的文本。

4. open()：打开一个新的浏览器窗口或标签页。

5. close()：关闭当前窗口。

6. setTimeout()：在指定的毫秒数后调用一个函数或执行一段代码。

7. setInterval()：每隔指定的毫秒数重复调用一个函数或执行一段代码。

8. clearTimeout()：取消setTimeout()设置的定时器。

9. clearInterval()：取消setInterval()设置的定时器。

10. window.innerWidth和window.innerHeight：获取窗口的内部宽度和高度（包括滚动条）。

11. window.outerWidth和window.outerHeight：获取窗口的外部宽度和高度（包括工具栏/滚动条）。

12. window.scrollX和window.scrollY：获取窗口的水平和垂直滚动位置。

13. window.localStorage：提供一种机制，可以让网页存储键值对到本地。

14. window.sessionStorage：提供一种机制，可以让网页存储会话期间的键值对。







## 栈内存和堆内存

栈内存 stack

堆内存 heap

- `原始类型`占据的空间在`栈内存`中分配  
- `对象类型`占据的空间在`堆内存`中分配

```javascript
var info = {
    name: "li",
    friend: {
        name: "wang"
    }
}

var friend = info.friend
friend.name = "jack"
console.log(info.friend.name) // jack
```

函数也保存在堆内存中

## this

函数中有一个this变量，this变量在大多数情况下指向一个变量

箭头函数没有this

this是在运行时被绑定的

### this指向

1. 如果普通的函数被默认调用，那么this指向的就是window

```javascript
function foo(){
    console.log(this)
}
foo() // window
```

2. 对象调用，this指向调用的对象

```javascript
function foo() {
    console.log(this)
}

var obj = {
    foo
}

obj.foo()  // 打印obj对象 { foo: [Function: foo] }
```

3. 通过call/apply调用

```javascript
function foo() {
    console.log(this)
}

foo.call("abc")  // String{"abc"}对象
```



```java
var obj = {
    bar: function() {
        console.log(this)
    }
}

var baz = obj.bar()
baz()  // window
```

4. 高阶函数

```javascript
function test1(){
    console.log(this);
}

function test2(fn){
    fn()
}

test2(test1)  // window
```







#### 严格模式

严格模式下，独立函数中的this指向的是undefined

```javascript
"use strict"
function foo() {
    console.log(this);
}
foo()  // undefined
```

#### 案例

```javascript
function test1(){
    console.log(this);
    test2()
}

function test2(){
    console.log(this);
}

test1() // window  window
```

### 显式绑定 call / apply / bind

执行函数，并且强制this就是obj对象

可以使用**call**，**apply**方法

隐式绑定必须在调用的对象内部有一个对函数的引用（比如一个属性）

```javascript
var obj = {
	name: "li"
}

function foo() {
    console.log(this)
}

foo.call(obj)
foo.apply(obj)
```

#### apply

func.apply(thisArg, [argsArray])

作用：apply方法会立即调用函数，并将this指向thisArg对象。同时，将argsArray中的元素作为函数的参数传入。

- thisArg：函数内部this要指向的对象。
- argsArray：一个数组或类数组对象，其中的元素将作为函数的参数传入。

```javascript
function foo(name, age, height) {
    console.log(this)
}

foo.apply("apply",["li", 18, 1.70])
```



#### call

语法：func.call(thisArg, arg1, arg2, ...)

作用：call方法会立即调用函数，并将this指向thisArg对象。同时，将arg1, arg2等参数依次传入函数。

- thisArg：函数内部this要指向的对象。
- arg1, arg2, ...：函数的参数，依次传入。

```javascript
function foo(name, age, height) {
    console.log(this)
}

foo.call("call", "li", 18, 1.70)
```



#### bind

创建一个新的函数，并将函数内部的this绑定到指定的对象上，但不会立即调用该函数。这样可以在需要的时候再调用新的函数。

语法：func.bind(thisArg, arg1, arg2, ...)

- thisArg：函数内部this要指向的对象。
- arg1, arg2, ...：预设的参数，调用新函数时这些参数会被传入。



```javascript
function foo(name, age, height) {
    console.log(this)
    console.log(name, age, height)
}

const obj = { name: "obj" }
const newFoo = foo.bind(obj, "li", 18)

newFoo(1.70) // 调用新函数，传入剩余的参数
```



### new绑定

1. 创建一个新对象
2. 新对象执行prototype连接
3. 新对象绑定到函数调用的this上
4. 函数没有返回其他对象，表达式返回这个新对象

```java
function foo() {
    console.log(this)
    this.name = "li"
}
new foo()  // {"name": "li"}
```



## 原始类型

### 原始类型包装类

JavaScript的原始类型并非对象类型，从理论上来说它们没有办法获取属性或者调用方法

```javascript
var message = "hello world"
var words = message.split(" ")
var length = message.length
```

 原始类型是<span style="color:red">简单的值</span>,默认不能调用属性和方法

JavaScript为了<span style="color:red">使其可以获取属性和调用方法，对其封装了对应的包装类型</span>



```javascript
var name = "li"
// 当调用 name.length 时
// 会生成一个对象
name = new String(name)

// 手动创建对象
var name = new String("li")
```



常见的包装类型有：String、Number、Boolean、Symbol、BigInt类型

## 包装类型

### 数字方法&属性

1. num.toString()

```javascript
var num = 1000
var str = num.toString()
var str1 = num.toString(2) // 转为2进制
```

2. num.toFixed(~)

将数字转换为指定小数位数的字符串表示

```javascript
let num = 3.14159;
console.log(num.toFixed(2));   // "3.14"
console.log(num.toFixed(4));   // "3.1416"
```

3. num.valueOf()

返回数字的原始值

```javascript
let num = 42;
console.log(num.valueOf());   // 42
```



### Number静态方法

1. Number.isInteger(value)

判断一个值是否为整数

```javascript
console.log(Number.isInteger(42));    // true
console.log(Number.isInteger(3.14));  // false
console.log(Number.isInteger("1"));  // false
```

2. Number.parseInt(string)

解析一个字符串并返回整数

```javascript
console.log(Number.parseInt('1.14g'))  // 1
console.log(parseInt('1.14g')) // 全局方法
```

3. Number.parseFloat(string)

解析一个字符串并返回浮点数

```javascript
console.log(Number.parseFloat('3.14'));    // 3.14
console.log(Number.parseFloat('314e-2'));  // 3.14
console.log(Number.parseFloat('3.14g'))    // 3.14
console.log(parseFloat('3.14g'))    // 全局方法
```

4. Number.isNaN(value)

判断一个值是否为NaN

```javascript
console.log(Number.isNaN(NaN));    // true
console.log(Number.isNaN(42));     // false
```



### 字符串方法&属性

1. str.length

获取长度

```js
const str = "hello",
str.length
```

2. str.charAt(~)  str.charAtCode(~)  

获取字符串指定位置的值 

charCodeAt()：该方法会返回指定索引位置字符的 Unicode 值，返回值是 0 - 65535 之间的整数

```js
const str = 'hello';
str.charAt(1)  // 输出结果：e 

let str = "abcdefg";
console.log(str.charCodeAt(1)); // "b" --> 98
```

3. str.split(~)

分割字符串

```js
let str = "Hello";
let s = str.split("e");
console.log(str); //Hello
console.log(s); //[ 'H', 'llo' ]
```

4. str.trim()

删除首尾空格

#### 获取子字符串

1. str.slice(~)

提取字符串某个部分，从start到end，不含end

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

2. str.subString(start, end)

不支持负值

3. str.substr(start, length)

从start开始获取长为length的字符串，允许start为负数

#### 字符串的拼接

一般使用 + 加号

1. str.concat(~)

```javascript
let str1 = "Hello, ";
let str2 = "world!";
let result = str1.concat(str2);
console.log(result); // 输出: "Hello, world!"

let str1 = "I ";
let str2 = "love ";
let str3 = "coding.";
let result = str1.concat(str2, str3);
// let result = str1.concat(str2).concat(str3);
console.log(result); // 输出: "I love coding."
```

2. str.padStart(~)

在字符串的开头填充另一个字符串,直到结果字符串达到给定的长度

```javascript
'abc'.padStart(5, 'x');  // 'xxabc'
'abc'.padStart(5, '0');  // '00abc'

// date.getMonth() 返回一个表示月份的数字,范围从0到11
String(date.getMonth() + 1).padStart(2, '0') // 如果月份为1，会变成01
```

3. a.localeCompare(b) 

比较字符串字母顺序

```js
let res = titleA.localeCompare(titleB);
```

- 如果 `titleA` 在字母顺序上小于 `titleB`，则返回负数。
- 如果 `titleA` 在字母顺序上大于 `titleB`，则返回正数。
- 如果 `titleA` 和 `titleB` 在字母顺序上相等，则返回零。

4. str.toLowerCase()

转小写

5. str.toUpperCase()

转大写

6. str1.indexof(str2)

判断一个字符串里是否含有另外一个字符串

搜索到，返回索引位置。

没搜索到返回 -1

```javascript
var message = "my name is li"
var name = "li"
console.log(message.indexOf(name)) // 11
console.log(message.indexOf("wang")) // -1
```

7. str.includes(searchString[ , position])

从位置 position 开始查找

- `searchString` 是必需的参数，表示要在字符串中搜索的子字符串。
- `[ , position]` 中的逗号和方括号表示 `position` 是一个可选的参数。

```javascript
let message = "Hello, world!";

console.log(message.includes("Hello")); // 输出: true
console.log(message.includes("hello")); // 输出: false
console.log(message.includes("world")); // 输出: true
console.log(message.includes("world", 8)); // 输出: false
```

8. str.startsWith(searchString[ , position])

从postiton开始，判断字符串是否以searchString开头

9. str.replace(regexp | substr, newSubStr | function)

查找到对应的字符串，并且使用新的字符串替代

也可以传入正则来查找，也可以传入一个函数来替换

```javascript
let text = "Hello, world!";
let newText = text.replace("world", "there");
console.log(newText); // 输出: "Hello, there!"

let text = "Apples and bananas";
let newText = text.replace(/a/g, "o");
console.log(newText); // 输出: "Apples ond bononos"

let text = "I have 2 cats and 3 dogs.";
let newText = text.replace(/\d+/g, (match) => parseInt(match) * 2);
console.log(newText); // 输出: "I have 4 cats and 6 dogs."
```





### 数组（Array）方法&属性

1. arr.at(i)

如果i >=0 arr[i]完全相同

如果i为负数，它从数组的尾部向前数

delete arr[i]（了解）

删除元素，该位置变为undefined

2. arr.push(~) 

向数组末尾添加

```js
const arr = [];
arr.push(3);
arr.push(2);
arr.push(1);
// [3, 2, 1]
```

3. arr.pop(~)

`arr.pop()` 方法用于移除数组的最后一个元素，并返回该元素

如果数组为空，`pop()` 返回 `undefined`

```javascript
let fruits = ["apple", "banana", "cherry"];
let lastFruit = fruits.pop();
console.log(lastFruit); // 输出: "cherry"
console.log(fruits);    // 输出: ["apple", "banana"]
```

4. arr.unshift(~)

将元素插入到数组的起始位置，并将其他元素向后移动

```js
const arr = [];
arr.unshift(3);
arr.unshift(2);
arr.unshift(1);
// [1,2,3]
```

5. arr.shift(~)

shift去除队列首端的一个元素，整个数组向前移动

```javascript
var names = ["John", "Mary", "Bob", "Tom"];
names.shift();
console.log(names); // [ 'Mary', 'Bob', 'Tom' ]
```

6. arr.splice(~)

在任意位置添加/删除/替换元素,原数组会被修改

```javascript
array.splice(start[ , deleteCount[, item1[ , item2[, ...]]]])
```

- **start**: 指定修改开始的索引。
- **deleteCount**: 要移除的元素数量。
- **items**: 要添加的新元素。

##### 移除元素

```javascript
let fruits = ["apple", "banana", "cherry", "date"];
let removed = fruits.splice(1, 2);
console.log(removed); // 输出: ["banana", "cherry"]
console.log(fruits);  // 输出: ["apple", "date"]
```

##### 添加元素

```javascript
let fruits = ["apple", "date"];
fruits.splice(1, 0, "banana", "cherry");
console.log(fruits); // 输出: ["apple", "banana", "cherry", "date"]
```

##### 替换元素

```javascript
let fruits = ["apple", "banana", "cherry"];
fruits.splice(1, 1, "orange");
console.log(fruits); // 输出: ["apple", "orange", "cherry"]
```

1. arr.slice(~)

<span style="color:red">提取数组的一部分,并返回一个新数组,不会修改原数组</span>

```javascript
const fruits = ['apple', 'banana', 'orange', 'mango'];
const slicedFruits = fruits.slice(1, 3);
console.log(slicedFruits); // 输出: ['banana', 'orange']
console.log(fruits); // 输出: ['apple', 'banana', 'orange', 'mango'] (原始数组未被修改)

let bottomColumn1 = bottomColumn.slice(0, -1);  // 除去最后一个元素
```

2. arr.concat(~)

<span style="color:red">合并两个或多个数组,并返回一个新数组,不会修改原数组</span>

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const concatenatedArr = arr1.concat(arr2);
console.log(concatenatedArr); // 输出: [1, 2, 3, 4, 5, 6]
console.log(arr1); // 输出: [1, 2, 3] (原始数组未被修改)
console.log(arr2); // 输出: [4, 5, 6] (原始数组未被修改)
```

3. arr.join(~)

<span style="color:red">将数组的所有元素连接成一个字符串</span>

```javascript
const animals = ['cat', 'dog', 'rabbit'];
const joinedString = animals.join('-');
console.log(joinedString); // 输出: 'cat-dog-rabbit'
console.log(animals); // 输出: ['cat', 'dog', 'rabbit'] (原始数组未被修改)
```

4. arr.find(~)

<span style="color:red">find()方法用于查找数组中满足提供的测试函数的第一个元素的值</span>

如果找到了满足条件的元素,find()方法会立即返回该元素的值。如果没有找到满足条件的元素,则返回undefined

```javascript
const numbers = [5, 12, 8, 130, 44];

// 查找第一个大于10的元素
const found = numbers.find(element => element > 10);
console.log(found); // 输出: 12
```

5. arr.findIndex(~)

与`find()`方法类似,区别是`find()`方法返回的是元素值,而`findIndex()`返回的是索引

```javascript
const numbers = [5, 12, 8, 130, 44];

const isLargeNumber = (element) => element > 13;

console.log(numbers.findIndex(isLargeNumber));
// 输出: 3
// 因为元素 130 满足条件,它的索引是 3
```

6. arr.indexof(~)

array.indexOf(searchElement[, fromIndex])

- `searchElement`:要查找的元素。
- `fromIndex`(可选):开始查找的索引位置,默认为0。如果为负数,则从数组末尾开始查找。

```javascript
const fruits = ['apple', 'banana', 'orange', 'banana'];

console.log(fruits.indexOf('banana')); // 输出: 1
console.log(fruits.indexOf('banana', 2)); // 输出: 3
console.log(fruits.indexOf('grape')); // 输出: -1
```

`indexOf()`方法使用严格相等(`===`)进行比较。它会区分数字和字符串,以及对象的引用。

7. arr.includes(~) 

<span style="color:red">判断是否有某个元素</span>

```js
const num1: number[] = [1, 2, 3, 4, 5];
console.log(num1.includes(1)); // 输出: true
console.log(num1.includes(6)); // 输出: false
```

8. arr.sort(~)

数组排序，修改原数组的同时返回一个数组

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

9. arr.reverse(~)

原地反转数组的元素顺序,并返回该数组的引用

```javascript
const arr = [1, 2, 3, 4, 5];
console.log(arr); 
// 输出: [1, 2, 3, 4, 5]

const reversedArr = arr.reverse();
console.log(reversedArr); 
// 输出: [5, 4, 3, 2, 1]

console.log(arr); 
// 输出: [5, 4, 3, 2, 1]
// 注意,原数组 arr 也被修改了
```

10. arr.forEach(~)

<span style="color:red">forEach方法用于遍历数组中的每个元素，并执行指定的操作</span>

forEach方法不会返回任何值，它只是用于执行某些操作

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

arr.forEach(function (item, index, arr) {
    console.log(item, index, arr)
})

const numbers = [1, 2, 3, 4, 5];
// 使用 forEach 修改原数组
numbers.forEach((num, index, arr) => {
  arr[index] = num * 2; // 修改原数组
});

console.log(numbers); // [2, 4, 6, 8, 10] （原数组被改变）
```

#### 手写forEach

版本一

```javascript
const arr = [1, 2, 3, 4, 5]

function hyForEach(fn, array){
    for(let i=0; i<array.length; i++){
        fn(arr[i], i, array)
    }
}


hyForEach((item,index,arr) => {
    console.log(item, index, arr);
}, arr)
```

版本二

```javascript
const arr = [1, 2, 3, 4, 5]

arr.hyForEach = function (fn) {
    for (let i = 0; i < this.length; i++) {
        fn(this[i], i, this)
    }
}

arr.hyForEach((item, index, arr) => {
    console.log(item, index, arr)
})
```

版本三

```javascript
Array.prototype.hyForEach = function(fn) {
    for(var i = 0; i < this; i++){
        fn(this[i], i, this)
    }
}
```



#### arr.map(~)

```js
const arr = [1, 2, 3, 4, 5]
const newArr = arr.map(item => item * 2)

arr.map((item,index,arr)=>{
    console.log(item,index,arr)
})


var n = arr.map(item => {
    item = item * 2
    console.log(item)
}).length  // 链式调用
```

- `map()` 方法会返回一个新的数组,该数组的元素是原始数组中的每个元素调用回调函数的结果。
- `forEach()` 方法没有返回值,它只是对数组中的每个元素执行回调函数,不会生成新的数组。

#### arr.filter(~)

<span style="color:red">不会修改原数组</span>

```js
const arr = [1, 2, 3, 4, 5]
const newArr = arr.filter(item => item % 2 === 0)  // [2,4]
```

在 `filter` 方法的回调函数中返回 `true` 或 `false` 是用于决定当前元素是否应该被保留在结果数组中

```typescript
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const newArr = arr.filter(num => {
    console.log(num);
    return num < 5;
});

console.log(newArr);  // [1,2,3,4]
```



#### arr.some(~)/arr.every(~)

```js
const arr = [1, 2, 3, 4, 5]

const flag = arr.some(item => item > 3)    // true
const flag1 = arr.every(item => item > 3)  //false
```

#### arr.reduce(~)

```js
arr.reduce((accumulator, currentValue, currentIndex, array) => {
    // 操作逻辑
}, init)
```

accumulator: 累加器，上一次调用的返回值

currentValue: 当前值，数组中当前被处理的元素

currentIndex:（当前索引，可选）,当前元素在数组中的索引。表示当前迭代到数组的第几个元素。

array:（原数组，可选）,调用 `reduce` 的数组本身

init:（初始值，可选），用于指定 `accumulator` 的初始值。如果不提供，`reduce` 会将数组的第一个元素作为 `accumulator` 的初始值，并从第二个元素开始迭代

##### 累加

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

```js
const data = ref([{ age: 18, name: '张三' }, { age: 20, name: '李四' }, { age: 25, name: '王五' }, { age: 30, name: '赵六' }])

let num = data.value.reduce((sum, cur) => {
    return sum + cur.age
}, 0)

//  简化
let num = data.value.reduce((sum, cur) =>sum + cur.age, 0)
```

##### 统计元素出现次数

```js
const fruits = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple'];

const count = fruits.reduce((acc, fruit) => {
    acc[fruit] = (acc[fruit] || 0) + 1;
    return acc;
}, {});

console.log(count); 
// 输出: { apple: 3, banana: 2, orange: 1 }
```





### Object方法&属性

#### Object.assign(~)

用于将一个或多个源对象的可枚举属性复制到目标对象，并返回修改后的目标对象

Object.assign(~)和扩展运算符都只能 浅拷贝，不会递归拷贝对象，如果对象里有嵌套对象，拷贝的仅是引用。

1. 合并对象属性

```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const mergedObj = Object.assign({}, obj1, obj2);
console.log(obj1); // { a: 1, b: 2 }
console.log(obj2); // { b: 3, c: 4 }
console.log(mergedObj); // { a: 1, b: 3, c: 4 }



const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const mergedObj = Object.assign(obj1, obj2);
console.log(obj1); // { a: 1, b: 3, c: 4 }
console.log(obj2); // { b: 3, c: 4 }
console.log(mergedObj); // { a: 1, b: 3, c: 4 }
```

2. 克隆对象

```javascript
const originalObj = { x: 1, y: 2 };
const clonedObj = Object.assign({}, originalObj);
console.log(clonedObj); // { x: 1, y: 2 }
```

3. 修改目标对象

```javascript
const targetObj = { a: 1 };
const sourceObj = { b: 2 };
Object.assign(targetObj, sourceObj);
console.log(targetObj); // { a: 1, b: 2 }
```

4. 如果源对象是从原型链继承的属性，`Object.assign` 会将这些属性合并到目标对象中

```javascript
const proto = { inherited: 42 };
const obj = Object.create(proto);
obj.own = 1;

const target = Object.assign({}, obj);
console.log(target); // { inherited: 42, own: 1 }


const proto = { inherited: 42 };
const obj = Object.create(proto);
obj.own = 1;

const target = { ...obj };
console.log(target); // { own: 1 }
```





#### Object.keys()

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

#### Object.values(~)

把对象的所有属性值写成一个数组

```js
const obj = {
    name: 'li',
    age: 20,
    gender: 'male'
}

const values = Object.values(obj);  // [ 'li', 20, 'male' ]
```

#### Object.entries()

`Object.entries()` 是 JavaScript 中的一个方法，用于返回一个对象自身可枚举属性的键值对数组。每个键值对都是一个包含两个元素的数组，第一个元素是属性名（键），第二个元素是属性值。

```javascript
const obj = { a: 1, b: 2, c: 3 };

const entries = Object.entries(obj);

console.log(entries);
// 输出: [ ['a', 1], ['b', 2], ['c', 3] ]
```





#### trailing comma

在 JavaScript 中，在数组或对象的最后一项后面添加一个逗号（trailing comma）是被允许的，并且不会导致语法错误。这种语法被称为 "trailing comma" 或 "final comma"

```javascript
const arr = [{
    width: 120,
    dataKey: "STATUS_MC",
    title: "状态",
    filter: true,
},]
```



## Math类型

### 方法

1. `Math.abs(x)`：返回一个数的绝对值。
2. `Math.ceil(x)`：向上取整，返回大于或等于一个数的最小整数。
3. `Math.floor(x)`：向下取整，返回小于或等于一个数的最大整数。
4. `Math.round(x)`：四舍五入取整，返回一个数字四舍五入后的整数。
5. `Math.max(x, y, z, ..., n)`：返回一组数中的最大值。
6. `Math.min(x, y, z, ..., n)`：返回一组数中的最小值。
7. `Math.pow(x, y)`：返回一个数的y次幂。
8. `Math.sqrt(x)`：返回一个数的平方根。
9. `Math.random()`：返回一个0到1之间的随机数（包括0但不包括1）。
10. `Math.sin(x)`：返回一个数的正弦值。
11. `Math.cos(x)`：返回一个数的余弦值。
12. `Math.tan(x)`：返回一个数的正切值。

### 属性

1. `Math.PI`：圆周率，约等于3.14159。
2. `Math.E`：自然对数的底数，约等于2.71828。

## 时间的表示方式

往东的时区（GMT+hh:mm）   往西的时区（GMT-hh:mm）

UTC（原子钟计算的标准时间）

### Date

```javascript
var date1 = new Date()
var date2 = new Date("2022-08-08") // 时间字符串
var date3 = new Date(2023, 10, 10, 09, 08, 07, 333) // 传入年月日时分秒毫秒
```

### Date获取unix时间戳

unix时间戳：是一个整数，是自1970年1月1日 

```javascript
const date = new Date(0)
console.log(date); // Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)
```

### dataString时间的表示方式

RFC 2822标准、ISO 8601 标准

- 默认打印时间格式是RFC 2822标准

```javascript
const date = new Date(0)
console.log(date); // Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)
```

- 可以转为 ISO 8601标准
  - YYYY：年份，0000 ~ 9999
  - MM：月份，01 ~12
  - DD：日，01 ~ 31
  - T：分隔日期和时间，没有特殊含义，可省略
  - HH：小时，00 ~24
  - mm：分钟，00 ~ 59
  - ss：秒，00~59
  - .sss：毫秒
  - Z：时区

```javascript
var date = new Date()
console.log(new Date())  // Wed Sep 25 2024 21:20:54 GMT+0800 (中国标准时间)
console.log(date.toDateString());  // Wed Sep 25 2024
console.log(date.toISOString());  // 2024-09-25T13:20:54.462Z
```

### Date获取信息

1. `getFullYear()`: 根据本地时间返回指定日期的年份(四位数年份)。
2. `getMonth()`: 根据本地时间返回指定日期的月份(0-11,0代表一月)。
3. `getDate()`: 根据本地时间返回指定日期一个月中的第几天(1-31)。
4. `getDay()`: 根据本地时间返回指定日期的星期几(0-6,0代表星期日)。
5. `getHours()`: 根据本地时间返回指定日期的小时(0-23)。
6. `getMinutes()`: 根据本地时间返回指定日期的分钟(0-59)。
7. `getSeconds()`: 根据本地时间返回指定日期的秒数(0-59)。
8. `getMilliseconds()`: 根据本地时间返回指定日期的毫秒(0-999)。
9. `getTime()`: 返回从1970年1月1日 00:00:00 UTC到指定日期的毫秒数。
10. `setMonth()`: 根据本地时间为指定日期设置月份(0-11)。
11. `setDate()`: 根据本地时间为指定日期设置月份中的第几天(1-31)。
12. `setHours()`: 根据本地时间为指定日期设置小时(0-23)。
13. `setMinutes()`: 根据本地时间为指定日期设置分钟(0-59)。
14. `setSeconds()`: 根据本地时间为指定日期设置秒(0-59)。
15. `setMilliseconds()`: 根据本地时间为指定日期设置毫秒(0-999)。
16. `setTime()`: 以毫秒数设置 Date 对象。

```javascript
let date = new Date("2014-01-08 14:05:06");
console.log("新创建的Date对象：", date);
 
// 1、把 Date 对象转换为字符串；
console.log("toString()方法，转换为字符串：", date.toString());
 
// 2、把 Date 对象的日期部分转换为字符串；
console.log("toDateString()方法，日期部分转换为字符串：", date.toDateString());
 
// 3、把 Date 对象的时间部分转换为字符串；
console.log("toTimeString()方法，时间部分转换为字符串：", date.toTimeString());
 
// 4、把 Date 对象，转换为本地时间格式的字符串；
console.log("toLocaleString()方法，转换为本地时间格式的字符串：", date.toLocaleString());
 
// 5、把 Date 对象的日期部分，转换为本地时间格式的字符串；
console.log("toLocaleDateString()方法，日期部分转换为本地时间格式的字符串：", date.toLocaleDateString());
 
// 6、把 Date 对象的时间部分，转换为本地时间格式的字符串；
console.log("toLocaleTimeString()方法，时间部分，转换为本地时间格式的字符串：", date.toLocaleTimeString());
 
// 7、把 Date 对象转换为JSON 数据格式字符串；
console.log("toJSON()方法，转换为JSON 数据格式字符串：", date.toJSON());
 
// 8、把 Date 对象转换为 ISO 标准的日期格式；
console.log("toISOString()方法，转换为 ISO 标准的日期格式：", date.toISOString());
```



### Date获取时间戳

```javascript
var date = new Date()
var timestamp = Date.now()
var timestamp1 = date.getTime()
var timestamp2 = date.valueOf()
```



```javascript
var timeString = "2023-06-01"

var date = new Date(timeString)
var timeStamp = date.getTime()

var timeStamp1 = Date.parse(timeString)
```



## Console

### console.dir()

用于输出一个对象的详细信息到控制台

```javascript
const person = {
  name: "John",
  age: 30,
  address: {
    city: "New York",
    country: "USA"
  }
};

console.log(person);
// 输出: [object Object]

console.dir(person);
// 输出:
// {
//   name: "John",
//   age: 30,
//   address: {
//     city: "New York",
//     country: "USA"
//   }
// }
```





## Dom

- 例子

 ```javascript
 var box1 = document.body.children[0]
 box1.style.color = "red"
 box1.children[0].style.fontSize = "100px"
 box1.children[0].nextElementSibling.style.color = "orange"
 ```

```javascript
const button = document.createElement('button')
button.textContent = 'Click me'
button.onclick = function () {
    console.log('Button clicked')
}
document.body.appendChild(button);
```



### document对象

对DOM的所有操作都是从document对象开始的

可以通过 window.document 或直接使用 document 来访问 document 对象

它是DOM的入口点，可以从document开始访问任何节点元素

- html元素：document.documentElement
- body元素：document.body
- head元素：document.head
- 文档声明：document.doctype

### 节点（Node）之间的导航

获取一个节点后，根据这个获取其他的节点

1. 获取子节点:
   - `childNodes`: 返回当前节点的所有子节点的 NodeList 对象。
   - `firstChild`: 返回当前节点的第一个子节点。 返回当前节点的第一个子节点,不论是元素节点还是文本节点或注释节点。
   - `lastChild`: 返回当前节点的最后一个子节点。
   - `children`: 返回当前节点的所有子元素节点的 HTMLCollection 对象。
   - `firstElementChild`: 返回当前节点的第一个子元素节点。只返回当前节点的第一个子元素节点,即具有标签名的节点。
   - `lastElementChild`: 返回当前节点的最后一个子元素节点。
2. 获取父节点:
   - `parentNode`: 返回当前节点的父节点。
   - `parentElement`: 返回当前节点的父元素节点。

### 获取元素

1. document.getElementById()

2. document.getElementsByClassName()

3. document.getElementsByName()
4. document.getElementsByTagName()
5. <span style="color:red">document.querySelector()</span>
6. <span style="color:red">document.querySelectorAll()</span>



### nodeType (了解)

- `Node.ELEMENT_NODE` (1)：表示元素节点，例如 `<div>`、`<p>` 等。
- `Node.TEXT_NODE` (3)：表示文本节点，即元素内的文本内容。
- `Node.COMMENT_NODE` (8)：表示注释节点，即 `<!-- 注释内容 -->`。
- `Node.DOCUMENT_NODE` (9)：表示文档节点，即整个 HTML 文档。

### data属性

```javascript
<body>
    <div data-content="hello" class="d1"></div>
</body>
<script>
var d1 = document.querySelector('.d1');
console.log(d1.dataset.content); // hello
```



### node属性

对象里的属性称为property，元素里的属性称为attribute

#### arrtibute操作

1. node.hasAttribute(~)：检查特性是否存在
2. node.getAttribute(~)
3. node.setAttribute(a,b)
4. node.removeAttribute(~)
5. node.attributes



```javascript
<div id="myDiv" class="example" data-custom="123">
        This is a div element.
</div>

<script>
    const div = document.getElementById('myDiv');

    // 检查属性是否存在
    console.log(div.hasAttribute('id')); // 输出: true

    // 获取属性值
    console.log(div.getAttribute('class')); // 输出: "example"
    console.log(div.getAttribute('data-custom')); // 输出: "123"

    // 设置属性值
    div.setAttribute('title', 'This is a tooltip');
    console.log(div.getAttribute('title')); // 输出: "This is a tooltip"

    // 移除属性
    div.removeAttribute('data-custom');
    console.log(div.hasAttribute('data-custom')); // 输出: false

    // 获取所有属性
    const attributes = div.attributes;
    console.log(attributes.length); // 输出: 3
    console.log(attributes[0].name); // 输出: "id"
    console.log(attributes[0].value); // 输出: "myDiv"
</script>
```





#### property操作

大部分情况推荐property方式，因为它默认情况下有类型

```javascript
const div = document.getElementById('myDiv');

// 使用 getAttribute() 方法获取 class 属性值
console.log(div.getAttribute('class')); // 输出: "example"

// 使用 className 属性获取 class 属性值
console.log(div.className); // 输出: "example"
```

可以通过修改class修改元素样式 

div.getAttribute('class') 和 div.className

1. `div.getAttribute('class')`：
   - 这是使用 `getAttribute()` 方法获取 `class` 属性的值。
   - 它返回一个字符串，表示元素的 `class` 属性值。
   - 如果元素没有 `class` 属性，则返回 `null`。
2. `div.className`：
   - 这是使用 `className` 属性直接访问元素的 `class` 属性值。
   - 它返回一个字符串，表示元素的 `class` 属性值。
   - 如果元素没有 `class` 属性，则返回一个空字符串 `""`。

#### 修改class

1. `elem.classList.add(class)`
2. `elem.classList.remove(class)`
3. `elem.classList.toggle(class)`：
   - 用于切换元素的 `class` 属性中指定的类名。
   - 如果该类名已经存在，则将其移除；如果该类名不存在，则将其添加。
   - 可以通过第二个可选参数 `force` 来控制切换行为。如果 `force` 为 `true`，则始终添加类名；如果 `force` 为 `false`，则始终移除类名。
4. `elem.classList.contains(class)`：
   - 用于检查元素的 `class` 属性中是否包含指定的类名。
   - 如果包含该类名，则返回 `true`；否则返回 `false`。
5. `elem.classList.replace(oldClass, newClass)`：
   - 用于将元素的 `class` 属性中的一个类名替换为另一个类名。
   - 如果 `oldClass` 存在，则将其替换为 `newClass`；如果 `oldClass` 不存在，则不进行任何操作。

#### 元素css属性

1. 对于多词属性，使用驼峰式

```javascript
boxEl.style.backgroundColor = "red"
boxEl.style = "font-size: 30px; color: red;"
boxEl.style.cssText = "font-size: 30px; color: red;"
```

2. 如果将值设为空字符串，会使用默认css样式



##### getComputedStyle(~)

读取元素style

```javascript
console.log(getComputedStyle(boxEl).fontSize)
```



##### data自定义属性

```javascript
<div id="abc" data-age="18" data-height="1.88"></div>
<script>
    var d1 = document.querySelector('#abc');
    console.log(d1.dataset.age); // 18
    console.log(d1.dataset.height); // 1.88
</script>
```





#### nodeName & tagName

nodeName：获取node节点的名字

tagName：获取元素的标签名词

对于元素，tagName和nodeName相同，对于其他节点只有nodeName



#### innerHTML & textContent

- 取值内容：
  - `innerHTML` 返回元素内部的 HTML 内容，包括所有子元素的 HTML 标签和文本内容。
  - `textContent` 返回元素内部的纯文本内容，不包括任何 HTML 标签，只返回所有子元素的文本内容。
- 赋值行为：
  - 当给 `innerHTML` 赋值时，赋值的字符串会被解析为 HTML 内容，并替换元素原有的内容。这意味着赋值的字符串中的 HTML 标签会被解释并生成相应的子元素。
  - 当给 `textContent` 赋值时，赋值的字符串会被视为纯文本，不会被解析为 HTML。它会替换元素原有的文本内容，且任何 HTML 标签都会被视为纯文本。

```javascript
<div>1<div>2</div></div>

var d1 = document.querySelector('div');
console.log(d1.innerHTML);  // 1<div>2</div>
console.log(d1.textContent); // 12
```



#### 其他属性

- value：input、select、textarea
- href：a
- 全局属性：id，class，titile，style，hidden



```javascript
var btn = document.querySelector(".btn")
btn.onclick = function(){}
```



### 创建元素

#### createElement

```javascript
var d1 = document.querySelector(".d1")
var h2 = document.createElement("h2")
h2.className = "title"
h2.textContent = "标题"

d1.append(h2)
```



### 插入元素

1. `appendChild()`：
   - 语法：`parentNode.appendChild(childNode)`
   - 将一个新的子节点添加到父节点的末尾。
   - 如果要插入的子节点已经存在于文档中，则会将其从原来的位置移动到新的位置。
2. `append()`：
   - 语法：`parentNode.append(node1, node2, ...)`
   - 在父节点的末尾插入一个或多个节点。
   - 可以插入元素节点、文本节点或字符串。
   - 如果要插入的节点已经存在于文档中，则会将其从原来的位置移动到新的位置。
3. `prepend()`：
   - 语法：`parentNode.prepend(node1, node2, ...)`
   - 在父节点的开头插入一个或多个节点。
   - 可以插入元素节点、文本节点或字符串。
   - 如果要插入的节点已经存在于文档中，则会将其从原来的位置移动到新的位置。
4. `before()`：
   - 语法：`referenceNode.before(node1, node2, ...)`
   - 在参考节点之前插入一个或多个节点。
   - 可以插入元素节点、文本节点或字符串。
   - 如果要插入的节点已经存在于文档中，则会将其从原来的位置移动到新的位置。
5. `after()`：
   - 语法：`referenceNode.after(node1, node2, ...)`
   - 在参考节点之后插入一个或多个节点。
   - 可以插入元素节点、文本节点或字符串。
   - 如果要插入的节点已经存在于文档中，则会将其从原来的位置移动到新的位置。

6. `replaceWith()`
   - 语法：`referenceNode.replaceWith(node1, node2, ...)`



### 移除元素

remove

```javascript
boxEl.remove()
```

### 复制元素

cloneNode()

```javascript
boxEl.cloneNode()
boxEl.cloneNode(true)  // 深度克隆,递归克隆节点的所有后代节点
```





### 事件 Event

#### input & change

- `input`事件在每次值发生变化时都会立即触发，因此它提供了实时的反馈。当你需要实时响应用户的输入并执行相应的操作时，可以使用 `input` 事件。
- `change` 事件只在值发生变化并失去焦点后触发，因此它提供了延迟的反馈。当你需要在用户完成输入后再执行相应的操作时，可以使用 `change` 事件。



#### mouseover & mouseenter

- mouseover和mouseout

  - 支持冒泡
  - 进入元素的子元素时
    - 先调用父元素的mouseout
    - 再调用子元素的mouseover
    - 因为支持冒泡，所以会将mouseover传递到父元素中

- mouseenter和mouseleave

  - 不支持冒泡

  - 进入子元素依然属于在该元素内，没有任何反应



#### 键盘事件

可以通过key和code来区分按下的键，keyCode已废弃

- key：字符（“A”，“a” 等）
- code：“按键代码” ，特定于键盘上按键的物理位置





#### 常见事件

1. 鼠标事件：
   - click：鼠标单击事件
   - dblclick：鼠标双击事件
   - mousedown：鼠标按下事件
   - mouseup：鼠标释放事件
   - mousemove：鼠标移动事件
   - mouseover：鼠标进入元素事件
   - mouseout：鼠标离开元素事件
   - contextmenu：鼠标右键菜单事件
   - mouseenter：鼠标指针移动到元素上（不支持冒泡）
   - mouseleave：鼠标指针移出元素（不支持冒泡）
2. 键盘事件：
   - keydown：键盘按下事件
   - keyup：键盘释放事件
   - keypress：键盘按压事件（已废弃，建议使用keydown或keyup）
3. 表单事件：
   - submit：表单提交事件
   - reset：表单重置事件
   - change：表单元素值改变事件
   - focus：元素获得焦点事件
   - blur：元素失去焦点事件
   - input：输入框内容改变事件
4. 文档/窗口事件：
   - load：页面或资源加载完成事件，文档中所有资源加载完毕
   - DOMContentLoaded：DOM内容加载完成事件，浏览器已完全加载HTML，并构建了DOM树，但是`<img>`和样式表之类的外部资源还没加载完
   - resize：窗口大小改变事件
   - scroll：滚动事件
   - unload：页面卸载事件
   - beforeunload：页面即将卸载事件
5. 动画事件：
   - animationstart：CSS动画开始事件
   - animationend：CSS动画结束事件
   - animationiteration：CSS动画重复播放事件
6. 过渡事件：
   - transitionstart：CSS过渡开始事件
   - transitionend：CSS过渡结束事件



#### 事件流

在浏览器上对着一个元素点击时，点击的不仅仅是这个元素本身。

HTML元素是存在父子元素叠加层级的。

事件冒泡：事件从最内层向外传递

事件捕获：从最外层到内层



#### 事件对象

event对象会在传入的事件处理回调函数时，被系统传入

```javascript
divEl.onclick = function(event){
    console.log(event)
}
```



event对象属性

1. `event.target`：触发事件的元素。在事件处理程序中，`this` 引用的是绑定事件的元素，而 `event.target` 引用的是实际触发事件的元素。
2. `event.currentTarget`：绑定事件的元素。在事件处理程序中，`event.currentTarget` 与 `this` 相同，引用的是绑定事件的元素。
3. `event.type`：事件的类型，如 `"click"`、`"mouseover"` 等。
4. `event.timestamp`：事件发生的时间戳。
5. `event.bubbles`：表示事件是否会冒泡。
6. `event.cancelable`：表示事件是否可以取消。
7. `event.defaultPrevented`：表示是否已经调用了 `event.preventDefault()`。
8. 鼠标事件相关属性：
   - `event.clientX` / `event.clientY`：鼠标指针相对于浏览器可视区域的坐标。
   - `event.pageX` / `event.pageY`：鼠标指针相对于整个文档的坐标。
   - `event.screenX` / `event.screenY`：鼠标指针相对于屏幕的坐标。
   - `event.offsetX` / `event.offsetY`：鼠标指针相对于触发事件的元素的坐标。
   - `event.button`：表示按下的鼠标按钮（0：左键，1：中键，2：右键）。
9. 键盘事件相关属性：
   - `event.key`：按下的键的字符串表示。
   - `event.code`：按下的键的物理键码。
   - `event.ctrlKey` / `event.shiftKey` / `event.altKey` / `event.metaKey`：表示是否按下了相应的修改键。
10. 触摸事件相关属性：
    - `event.touches`：包含所有当前触摸点的 Touch 对象数组。
    - `event.targetTouches`：包含在当前目标元素上的触摸点的 Touch 对象数组。
    - `event.changedTouches`：包含触发当前事件的触摸点的 Touch 对象数组。
11. 阻止事件：
    - `event.preventDefault()`：阻止事件的默认行为。
    - `event.stopPropagation()`：阻止事件的冒泡。



#### EventTarget

所有的节点、元素继承自EventTarget类

EventTarget是一个DOM接口，主要用于添加、删除、派发Event事件。

##### 浏览器环境的js对象层次

1. `Object`：所有对象的基类。
2. `EventTarget`：继承自 `Object`，是所有可以接收事件的对象的基类。
3. `Node`：继承自 `EventTarget`，是 DOM 节点的基类。
4. `Element`、`Document`、`Window` 等：继承自 `Node`，分别表示元素节点、文档节点和窗口对象。



其中，`Window` 对象是浏览器中最顶级的对象，它代表了浏览器窗口，并且是 JavaScript 代码执行的全局环境。`window` 对象继承自 `EventTarget`，因此它具有 `EventTarget` 的所有属性和方法，可以用于添加和管理事件监听器。



##### EventTarget 方法 & 属性

1. `addEventListener(type, listener[, options])`：将指定的事件监听器注册到 `EventTarget` 上。
   - `type`：事件的类型，如 `"click"`、`"mouseover"` 等。
   - `listener`：事件处理函数。
   - options：可选的配置对象，包含以下属性：
     - `capture`：表示是否在捕获阶段触发事件，默认为 `false`。
     - `once`：表示事件监听器是否只触发一次，默认为 `false`。
     - `passive`：表示事件监听器是否为被动的，即不会调用 `preventDefault()`，默认为 `false`。
2. `removeEventListener(type, listener[, options])`：从 `EventTarget` 中移除指定的事件监听器。
   - 参数与 `addEventListener()` 相同。
3. `dispatchEvent(event)`：将指定的事件分派到 `EventTarget` 上。
   - `event`：要分派的事件对象，通常是使用 `Event` 构造函数创建的。



```javascript
winodw.dispatchEvent(new Event("newEvent"))

window.addEventListener("newEvent",function(){
    console.log("监听到事件")
})
```



#### 事件委托

当子元素被点击，父元素可以通过事件冒泡监听到子元素的点击

可以通过event.target获取到当前监听的元素



- 案例

点击ul中某一个li，li变红色

ul>li{$}*10

```html
<!DOCTYPE html>
<html lang="en" style="height: 3000px;">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
    </style>
</head>

<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ul>
</body>
<script>

    var liEls = document.querySelectorAll('li');
    for (var liEl of liEls) {
        liEl.addEventListener('click', function () {
            this.style.color = 'red';
        });
    }

</script>

</html>
```

## BOM

浏览器对象模型(Browser Object Model)

- 简称BOM，由浏览器提供的用于处理文档之外的所有对象内容

比如`navigator`、`location`、`history`等对象



### window对象

ESMAScript规范：全局对象  -> globalThis

对于浏览器 -> window

对于node -> global 



- 使用var变量定义的变量会被添加到window对象中
- 放在window对象上的所有属性都可以被访问
- window默认给我们提供了全局函数和类：setTimeout、Math、Date、Object等



#### window对象常见属性

1. `window.document`: 对Document对象的只读引用。
2. `window.location`: 用于窗口或框架的 Location 对象。
3. `window.history`: 对 History 对象的只读引用。
4. `window.console`: 对 Console 对象的只读引用，用于提供对浏览器调试控制台的访问。
5. `window.screen`: 对 Screen 对象的只读引用。
6. `window.navigator`: 对 Navigator 对象的只读引用。
7. `window.innerWidth`、`window.innerHeight`: 获取浏览器窗口的内部宽度和高度(以像素为单位)，包括垂直和水平滚动条的大小。
8. `window.outerWidth`、`window.outerHeight`: 获取整个浏览器窗口的宽度和高度，包括侧边栏（如果存在）、窗口镶边（window chrome）和调正窗口大小的边框。
9. `window.pageXOffset`、`window.pageYOffset`: 获取文档从窗口左上角沿着 X 轴、Y 轴滚动的像素。
10. `window.localStorage`、`window.sessionStorage`: 允许在浏览器中存储key/value对的对象。localStorage 里面的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。
11. `window.parent`、`window.top`: parent属性返回父窗口，top属性返回最顶层窗口。
12. `window.name`: 获取/设置窗口的名称。
13. `window.closed`: 表明引用的窗口是否已经关闭。
14. `window.frames`: 返回一个类数组对象，列出了当前窗口中所有的子框架。



#### window对象常见方法

1. `window.alert()`: 显示带有一段消息和一个确认按钮的警告框。
2. `window.confirm()`: 显示一个带有指定消息和确认及取消按钮的对话框。
3. `window.prompt()`: 显示可提示用户输入的对话框。
4. `window.open()`: 打开一个新的浏览器窗口或查找一个已命名的窗口。
5. `window.close()`: 关闭浏览器窗口。
6. `window.moveTo()`: 把窗口的左上角移动到一个指定的坐标。
7. `window.resizeTo()`: 动态调整窗口的大小。
8. `window.scrollTo()`: 将文档滚动到指定位置。
9. `window.getComputedStyle()`: 获取指定元素的最终样式信息。
10. `window.matchMedia()`: 返回一个新的MediaQueryList对象，表示指定的媒体查询字符串解析后的结果。
11. `window.requestAnimationFrame()`: 告诉浏览器您希望执行动画并请求浏览器调用指定的函数在下一次重绘之前更新动画。
12. `window.cancelAnimationFrame()`: 取消一个先前通过调用window.requestAnimationFrame()方法添加到计划中的动画帧请求。
13. `window.setTimeout()`: 设置一个定时器，该定时器在定时器到期后执行一个函数或指定的一段代码。
14. `window.clearTimeout()`: 取消由 setTimeout() 方法设置的定时器。
15. `window.setInterval()`: 重复调用一个函数或执行一个代码片段，在每次调用之间具有固定的时间延迟。
16. `window.clearInterval()`: 取消由 setInterval() 设置的定时器。
17. `window.requestIdleCallback()`: 将一个函数排队，以便在浏览器空闲时期被调用。这使开发者能够在主事件循环上执行后台和低优先级工作，而不会影响延迟关键事件。
18. `window.cancelIdleCallback()`: 取消由 `window.requestIdleCallback()` 创建的回调。
19. `window.getSelection()`: 返回一个 Selection 对象，表示用户选择的文本范围或光标的当前位置。
20. `window.print()`: 打开打印对话框以打印当前文档。



#### window对象常见事件

1. `load`: 当整个页面及所有依赖资源如样式表和图片都已完成加载时，将触发load事件。
2. `unload`: 当用户导航离开页面时触发unload事件。
3. `beforeunload`: 在窗口即将被卸载时触发，可以用来弹出确认对话框，让用户确认是否离开当前页面。
4. `error`: 当发生JavaScript错误时触发error事件。
5. `resize`: 当浏览器窗口的大小被改变时触发resize事件。
6. `scroll`: 当文档视图或某个元素已经滚动时触发scroll事件。
7. `focus`: 当窗口获得焦点时触发focus事件。
8. `blur`: 当窗口失去焦点时触发blur事件。
9. `hashchange`: 当URL的hash部分(#后面的部分，包括#)发生变化时触发hashchange事件。
10. `popstate`: 当浏览器历史记录发生变化时触发popstate事件。
11. `online`: 当浏览器开始在线工作时触发online事件。
12. `offline`: 当浏览器开始离线工作时触发offline事件。
13. `message`: 当窗口接收到一个消息时触发message事件，通常用于不同源的窗口之间的通信。
14. `storage`: 当Web Storage(localStorage或sessionStorage)发生变化时触发storage事件。
15. `pageshow`: 当页面显示时触发，无论该页面是否来自缓存。
16. `pagehide`: 当页面隐藏时触发，无论该页面是否被缓存。
17. `beforeprint`: 当页面即将开始打印或预览时触发。
18. `afterprint`: 当页面已经开始打印，或者打印预览已经关闭时触发。
19. `contextmenu`: 当用户尝试打开上下文菜单时触发。
20. `wheel`: 当用户使用鼠标滚轮或类似的输入设备时触发。



### location对象

表示window上当前链接到的URL信息

#### 常见的属性

1. `location.href`: 返回当前页面的完整URL，即地址栏中显示的URL。
2. `location.protocol`: 返回URL的协议部分，包括冒号，例如"http:"或"https:"。
3. `location.host`: 返回URL的主机部分，包括端口号（如果有）。
4. `location.hostname`: 返回URL的主机名部分，不包括端口号。
5. `location.port`: 返回URL的端口号部分。如果URL中不包含明确的端口号，则返回空字符串。
6. `location.pathname`: 返回URL的路径部分，开头有一个 "/" 。
7. `location.search`: 返回URL的查询字符串部分，开头有一个"?"。
8. `location.hash`: 返回URL的片段标识符部分，开头有一个"#"。
9. `location.origin`: 返回URL的源，包括协议、主机名和端口（如果有）。
10. `location.ancestorOrigins`: 返回一个 DOMStringList 对象，包含了当前页面所有祖先浏览上下文的源。



<span style = "color:red">URL: `https://www.example.com:8080/search?q=javascript&sort=desc#results`</span>

1. `location.href`: `https://www.example.com:8080/search?q=javascript&sort=desc#results`
   - 返回完整的URL。
2. `location.protocol`: "https:"
   - 返回URL的协议部分,包括冒号。
3. `location.host`: `www.example.com:8080`
   - 返回URL的主机部分,包括端口号。
4. `location.hostname`: `www.example.com`
   - 返回URL的主机名部分,不包括端口号。
5. `location.port`: "8080"
   - 返回URL的端口号部分。如果URL使用默认端口(如https的443),则返回空字符串。
6. `location.pathname`: "/search"
   - 返回URL的路径部分,开头有一个"/"。
7. `location.search`: "?q=javascript&sort=desc"
   - 返回URL的查询字符串部分,开头有一个"?"。
8. `location.hash`: "#results"
   - 返回URL的片段标识符部分,开头有一个"#"。
9. `location.origin`: `https://www.example.com:8080`
   - 返回URL的源,包括协议、主机名和端口(如果有)。
10. `location.ancestorOrigins`: 取决于具体情况
    - 如果这个URL是在一个iframe中,且这个iframe有父级框架,`ancestorOrigins`会包含所有父级框架的源。
    - 如果这个URL是在主文档中,或者没有父级框架,`ancestorOrigins`将会是一个空的`DOMStringList`。



#### 常见的方法

1. `location.assign(url)`: 加载给定URL的新文档,就像点击一个链接一样。这将创建一个新的历史记录条目。
2. `location.replace(url)`: 用给定的URL替换当前文档。这不会在历史记录中创建新条目,所以用户无法通过"后退"按钮返回到原始文档。
3. `location.reload(forcedReload)`: 重新加载当前文档。如果参数`forcedReload`是`true`,浏览器将从服务器重新加载文档,而不是从缓存中加载。
4. `location.toString()`: 返回Location对象的字符串表示,等同于`location.href`。
5. `location.ancestorOrigins.item(index)`: 返回指定索引处的祖先源。这是`ancestorOrigins` DOMStringList的一部分。



### URLSearchParams

- 可以将一个字符串转为URLSearchParams类型
- 也可以把一个URLSearchParams类型转为字符串

```javascript
// 创建一个新的URLSearchParams对象
const params = new URLSearchParams("key1=value1&key2=value2");

// 获取一个参数的值
console.log(params.get("key1")); // "value1"

// 添加一个新的参数
params.append("key3", "value3");

// 将参数转换为字符串
console.log(params.toString()); // "key1=value1&key2=value2&key3=value3"
```



#### 常见的方法

1. `append(name, value)`: 追加一个新值到指定参数。

   ```javascript
   const params = new URLSearchParams("key1=value1");
   params.append("key1", "value2");
   console.log(params.toString()); // "key1=value1&key1=value2"
   ```

2. `delete(name)`: 删除指定的参数。

   ```javascript
   const params = new URLSearchParams("key1=value1&key2=value2");
   params.delete("key1");
   console.log(params.toString()); // "key2=value2"
   ```

3. `get(name)`: 返回指定参数的第一个值。

   ```javascript
   const params = new URLSearchParams("key1=value1&key1=value2");
   console.log(params.get("key1")); // "value1"
   ```

4. `getAll(name)`: 以数组的形式返回指定参数的所有值。

   ```javascript
   const params = new URLSearchParams("key1=value1&key1=value2");
   console.log(params.getAll("key1")); // ["value1", "value2"]
   ```

5. `has(name)`: 如果指定的参数存在,返回true。

   ```javascript
   const params = new URLSearchParams("key1=value1");
   console.log(params.has("key1")); // true
   console.log(params.has("key2")); // false
   ```

6. `set(name, value)`: 设置一个参数的值,如果参数已存在,替换其值,否则添加一个新参数。

   ```javascript
   const params = new URLSearchParams("key1=value1");
   params.set("key1", "newValue");
   console.log(params.toString()); // "key1=newValue"
   ```

7. `sort()`: 按键名对所有参数进行排序。

   ```javascript
   const params = new URLSearchParams("c=3&a=1&b=2");
   params.sort();
   console.log(params.toString()); // "a=1&b=2&c=3"
   ```

8. `forEach(callback, thisArg)`: 对每个参数执行一次给定的函数。

   ```javascript
   const params = new URLSearchParams("key1=value1&key2=value2");
   params.forEach((value, key) => {
     console.log(key + ": " + value);
   });
   // 输出:
   // "key1: value1"
   // "key2: value2"
   ```

9. `entries()`: 返回一个iterator对象,允许迭代访问所有的参数,每个参数以[key, value]的形式返回。

   ```javascript
   const params = new URLSearchParams("key1=value1&key2=value2");
   for (const [key, value] of params.entries()) {
     console.log(key + ": " + value);
   }
   ```

10. `keys()`: 返回一个iterator对象,允许迭代访问所有参数的键名。

    ```javascript
    const params = new URLSearchParams("key1=value1&key2=value2");
    for (const key of params.keys()) {
      console.log(key);
    }
    ```

11. `values()`: 返回一个iterator对象,允许迭代访问所有参数的值。

    ```javascript
    const params = new URLSearchParams("key1=value1&key2=value2");
    for (const value of params.values()) {
      console.log(value);
    }
    ```

12. `toString()`: 返回查询参数的字符串表示,可直接使用于URL。

    ```javascript
    const params = new URLSearchParams("key1=value1&key2=value2");
    console.log(params.toString()); // "key1=value1&key2=value2"
    ```

​		

### histroy对象

前端路由核心：修改了URL，页面不刷新，没有从服务器请求

#### 常见的属性

1. history.length: 返回一个整数,表示会话历史中元素的数目,包括当前加载的页面。例如,在一个新的选项卡加载的一个页面中,这个属性返回 1。
2. history.state: 返回一个表示历史堆栈顶部的状态的值。这是一种可以不必等待 popstate 事件而查看状态的方式。
3. history.scrollRestoration: 允许 web 应用程序在历史导航上显式地设置默认滚动恢复行为。此属性可以是自动的 ( auto ) 或者手动的 ( manual )。

#### 常见的方法

1. back(): 加载 history 列表中的前一个 URL。作用和浏览器回退按钮一样。语法:

   ```javascript
   window.history.back();
   ```

2. forward(): 加载 history 列表中的下一个 URL。作用和浏览器前进按钮一样。语法:

   ```javascript
   window.history.forward();
   ```

3. go(): 通过当前页面的相对位置从浏览器历史记录( session history )加载页面。比如 go(-1) 相当于 back(),go(1) 相当于 forward()。语法:

   ```javascript
   window.history.go(number);
   ```

4. pushState(): 在浏览历史中添加一个新记录。这个方法接收三个参数:状态对象, 标题 (目前被忽略), 和 (可选的) 一个URL。使用举例:

   ```javascript
   const state = { 'page_id': 1, 'user_id': 5 }
   const title = ''
   const url = 'hello-world.html'
   history.pushState(state, title, url)
   ```

5. replaceState(): 修改当前的历史记录,其参数与 pushState() 相同。使用举例:

   ```javascript
   const state = { 'page_id': 1, 'user_id': 5 }
   const title = ''
   const url = 'hello-world.html'
   history.replaceState(state, title, url)
   ```

6. state 属性: 返回一个表示历史堆栈顶部的状态的值。这是一种可以不必等待 popstate 事件而查看状态的方式。

   ```javascript
   let currentState = history.state
   ```



### navigator

### screen



## 窗口大小

BOM（浏览器对象模型）：

1. window.innerWidth：浏览器窗口的内部宽度，包括滚动条宽度（如果存在）。
2. window.innerHeight：浏览器窗口的内部高度，包括滚动条高度（如果存在）。
3. window.outerWidth：浏览器窗口的外部宽度，包括工具栏和滚动条。
4. window.outerHeight：浏览器窗口的外部高度，包括工具栏和滚动条。
5. window.screenX、window.screenLeft：浏览器窗口相对于屏幕左边缘的水平距离。
6. window.screenY、window.screenTop：浏览器窗口相对于屏幕上边缘的垂直距离。
7. window.scrollX、window.pageXOffset：文档水平滚动的像素数。
8. window.scrollY、window.pageYOffset：文档垂直滚动的像素数。



滚动方法：

1. scrollBy(x, y)：将页面滚动至相对于当前位置的（x，y）位置
2. scrollTo(x, y)：将页面滚动至绝对坐标



DOM（文档对象模型）：

1. document.documentElement.clientWidth：文档可视区域的宽度，不包括滚动条。
2. document.documentElement.clientHeight：文档可视区域的高度，不包括滚动条。
3. document.documentElement.offsetWidth：文档根元素的宽度，包括内容宽度、内边距和边框宽度。
4. document.documentElement.offsetHeight：文档根元素的高度，包括内容高度、内边距和边框高度。
5. document.documentElement.scrollWidth：文档内容的总宽度，包括溢出的部分。
6. document.documentElement.scrollHeight：文档内容的总高度，包括溢出的部分。
7. document.body.clientWidth：文档body元素的可视区域宽度，不包括滚动条和边框。
8. document.body.clientHeight：文档body元素的可视区域高度，不包括滚动条和边框。
9. document.body.offsetWidth：文档body元素的宽度，包括内容宽度、内边距和边框宽度。
10. document.body.offsetHeight：文档body元素的高度，包括内容高度、内边距和边框高度。
11. document.body.scrollWidth：文档body元素内容的总宽度，包括溢出的部分。
12. document.body.scrollHeight：文档body元素内容的总高度，包括溢出的部分。
13. element.getBoundingClientRect()：返回元素的大小及其相对于视口的位置。



## 浏览器JS原理

### 浏览器内核

排版引擎也称浏览器引擎、页面渲染引擎

- 常见的浏览器内核

  1. Blink内核

     - 由Google开发,基于WebKit内核

     - 应用于Google Chrome、新版Microsoft Edge、Opera等浏览器

     - 占据当前浏览器市场份额最大,约70%左右

  2. WebKit内核

     - 应用于Safari、iOS内置浏览器、360极速浏览器、搜狗高速浏览器、移动端浏览器（Android、iOS）等

     - 曾经也被Chrome等浏览器使用,后来Chrome改用Blink

  3. Gecko内核

     - 应用于Firefox浏览器

     - 完全开源,支持W3C标准

  

  <img src="./img/llq.png" align="left"/>

  

- link元素不会阻塞DOM Tree构建过程，但是会阻塞Render Tree构建过程



### 回流和重绘

#### 回流reflow

- 第一次确定节点大小和位置称之为布局（layout）
- 之后对节点大小、位置修改重新计算称为回流
  - DOM结构发生改变（添加、移除节点）
  - 改变了布局
  - 窗口resize
  - 调用getComputedStyle方法获取尺寸、位置信息



#### 重绘repaint

- 第一次渲染内容称之为绘制
- 之后重新渲染称之为重绘
- 修改背景色、文字颜色、边框颜色、样式等
- 回流一定会引起重绘，要经量避免回流



### composite合成



### script元素的处理

- 在浏览器解析HTML过程中，遇到了script元素不能继续构建DOM树
- 它会停止构建，首先下载JavaScript代码，并且执行JavaScript脚本
- 等到JavaScript脚本执行结束，才会继续解析HTML，构建DOM树
- 等到DOM树构建完成并渲染再执行JavaScript会造成严重的回流和重绘

为了解决这个问题，script元素给我们提供了两个属性：`defer`和`async`



#### defer属性

`async`属性允许脚本异步下载和执行,不会阻塞DOM树的构建和页面渲染。`defer`属性也允许脚本异步下载,但要等到DOM树构建完成后才执行。

```html
<script src="./js/test.js" defer></script>
```

告诉浏览器不要等待脚本下载，继续解析HTML，构建DOM Tree



#### async属性

- async脚本不能保证顺序，它是独立下载，独立运行，不会等待其他脚本
- async不会能保证在DOMContentLoaded之前或之后执行



### JavaScript运行原理

#### JavaScript代码执行

- 浏览器内核由两部分组成，例如webkit
  - WebCore：负责HTML解析、布局、渲染等工作
  - JavaScriptCore：解析、执行JavaScript代码。

##### V8引擎

用于Chrome，Node.js等

可以独立运行，也可以嵌入任何C++应用程序



## 原型

JavaScript当中每个对象都有一个特殊的内置属性`[[prototype]]`，这个特殊的对象可以指向另外一个对象

### 对象原型

```javascript
console.log(obj._proto_)  
console.lof(obj.getPrototypeOf(obj))  
```



- 当我们通过对象的属性key来获取一个value时，它会触发[[Get]]的操作
  - 它会优先在自己的对象中查找，如果找到直接返回
  - 如果没有找到，那么会访问对象`[[prototype]]`内置属性指向的对象上的属性



### 函数原型 prototype

所有的函数都有一个prototype属性，箭头函数没有

- 作用：用来构建对象时，给对象设置隐式原型

- new操作

  ```javascript
  new Person()
  ```

  1. 创建空对象

     ```javascript
     var obj = {}
     ```

  2. 将这个空对象赋值给this

     ```javascript
     
     ```

  3. 对象内部的[[prototype]]属性（隐式原型）会被赋值为该构造函数的prototype属性（显式原型）

     ```javascript
     obj._proto_ = Person.prototype
     ```

     

所有的对象都是通过new函数创建的

```javascript
var obj = { a: 1 }  // 是一个语法糖，本质上是
var obj = new Object();
obj.a = 1;
```

所有的函数也是对象，本质上是 new Function创建

- 函数可以有属性 

- 所有对象都是引用类型，保存的是地址

```javascript
function foo() {}
// 等价于
var foo = new Function(); // 创建一个空函数

var sum = new Function('a', 'b', 'return a + b');
console.log(sum(1, 2)); // 3
```

箭头函数没有this，没有prototype属性，箭头函数的创建不是通过 `Function` 构造函数

Function函数不是通过new创建的是直接放在js内存里



默认情况下，property是一个普通的Object对象。

默认情况下，property中有一个属性，constructor，它也是一个对象，它指向构造函数本身。

```javascript
function test() {
    return {}
}

console.log(test.prototype.constructor === test);  // true
Object.prototype.constructor === Object  // true
```

```javascript
Array.prototype.abc = 123;
console.log([].abc);  // 123
console.log([1, 2, 3].abc);  // 123
// 可以通过prototype加入新方法
```



### 隐式原型

所有对象都有一个属性: `__protp__`,称之为隐式原型

默认情况下，隐式原型指向创建该对象的函数的原型

当访问一个对象的成员时：

1. 看该对象自身是否拥有该成员，如果有直接使用
2. 看该对象的隐式原型是否拥有该成员，如果有直接使用
3. 在原型链中依次查找





### 原型链

所有函数（除箭头函数）都有call，apply，bind。存在于Function的原型里

```javascript
var A = new Function();
A.__proto__ === Function.prototype
```

Object的隐式原型是Fuction函数的原型，Object的prototype是Object原型。

1. Function的`__proto__`指向自身的`prototype`

2. Object的`prototype`的`__proto__`指向null



所有的函数对象都是 `Function` 的实例，因此它们继承了 `Function.prototype` 的属性和方法。同时，由于 `Function.prototype` 继承自 `Object.prototype`，函数对象也间接继承了 `Object.prototype` 的属性和方法。

![yx](.\img\yx.jpg)



```javascript
var obj = {}
obj.toString()

```

在 JavaScript 中，所有的对象都继承自 `Object` 原型（prototype）。`Object` 原型定义了一些基本的方法，其中就包括 `toString()` 方法。因此，所有的对象都拥有 `toString()` 方法，包括通过对象字面量创建的空对象 `{}`。

当你调用 `obj.toString()` 时，实际上是调用了 `Object.prototype.toString()` 方法。这个方法的默认实现返回一个字符串 `"[object Object]"`，表示该对象是一个普通的对象。

你可以通过覆盖 `toString()` 方法来自定义对象的字符串表示形式。许多内置对象，如 `Array`、`Date`、`Number` 等，都重写了 `toString()` 方法，以提供更有意义的字符串表示。

```javascript
var arr = [1, 2, 3];
console.log(arr.toString()); // 输出: "1,2,3"
// arr调用Object的toString
Object.prototype.toString.call(arr)  // "[Object Array]"


var date = new Date();
console.log(date.toString()); // 输出: 当前日期和时间的字符串表示

var num = 42;
console.log(num.toString()); // 输出: "42"
```



### 面试题

```javascript
function User() {}
User.prototype.sayHello = function() {}

const u1 = new User()
const u2 = new User()

console.log(u1.sayHello === u2.sayHello);  // true
console.log(User.prototype.constructor);  // [Function: User]
console.log(User.prototype === Function.prototype);  // false
console.log(User.__proto__ === Function.prototype);  // true
console.log(User.__proto__ === Function.__proto__);  // true
console.log(u1.__proto__ === u2.__proto__);  // true
console.log(u1.__proto__ === User.__proto__);  // false
console.log(Function.__proto__ === Object.__proto__);  // true
console.log(Function.prototype.__proto__ === Object.prototype.__proto__);  // false
console.log(Function.prototype.__proto__ === Object.prototype);  // true
```











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

```javascript
console.log(new Date());  // Sun Oct 06 2024 13:08:40 GMT+0800 (中国标准时间)
console.log(new Date().getTime());  // 1728191320324
```



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


const date2 = new Date().getTime() - 1000 * 60 * 60 * 24 * 30; // 得到时间戳
const dateObject = new Date(date2); // 转回 Date 类型
```



##  == 和 === 的区别

相等运算符(==)会进行类型的转换

全等运算符(===)不会进行类型的转换

`undefined` 只与 `null` 和它自身相等

`false` 在与其他类型比较时，会先将其他类型转换为布尔值，然后进行比较

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
undefined == false // false
null == 0 // false
undefined == '' //false 

'false' == false // false  false会被转为0
'0' == false  // true
NaN == NaN // false
NaN == false // false
NaN === false // false
```



```javascript
var info = {
    name: "li",
    age: 18,
    [Symbol.toPrimitive](){
        return 123
    }
}

console.log(123 == info)  // true

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



### NaN

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



## 防抖

`防抖策略`是当事件被触发后，`延迟n秒`再`执行回调`，如果在这n秒内时间被触发重新计时

### 应用场景

1. 用户在输入框中连续输入一串字符时，可以通过防抖策略，只有输入完后，才执行查询的请求。



### 实现

1. 通过timer清空

```javascript
import { ElButton, ElInput } from 'element-plus';
import { defineComponent, nextTick, onMounted, ref } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {

        let timer: NodeJS.Timeout | null = null;
        const print = () => {
            clearTimeout(timer!);
            timer = setTimeout(() => {
                console.log('print');
            }, 3000);
        }


        return () => (
            <div>
                <ElButton onClick={print}></ElButton>
            </div>
        );
    }
});
```

2. 导入Underscore.js



## 节流 throttle函数

- 如果事件被频繁触发，节流函数会按照一定的频率来执行函数
- 不管中间有多少次触发事件，执行函数的频率固定

- 类似于飞机大战，即使按下的频率非常快，子弹也保持一定速度发射



### 实现

1. 使用Underscore.js里的throttle方法

2. 手写

```javascript
function hythrottle(fn, interval) { // 执行函数，时间间隔
    let startTime = 0;
    
    const _throttle = function(...args) {
        const nowTime = new Date().getTime()
        const waitTime = interval - (nowTime - startTime)
        if(waitTime <= 0) {
            fn.apply(this,args)
            startTime = nowTime
        }
    }
    return _throttle
}
```





## 事件总线

```javascript
const info = {
	name : "li",
    age: 18,
    friend: {
    	name: "zhang"
	}
}

// 1.引用赋值
const obj1 = info

// 2.浅拷贝
const obj2 = {...info}
obj2.friend.name = "james"
console.log(info.friend.name)  // james

const obj3 = Object.assign({},info)
obj3.friend.name = "james"
console.log(info.friend.name)  // james

// 3.深拷贝
const obj4 = JSON.parse(JSON.stringify(info))  // 对于函数，Symobl无法处理
obj4.friend.name = "curry"
console.log(info.friend.name)  // zhang
```



### 浅拷贝

- 浅拷贝是创建一个新对象，但是新对象的属性值是原始对象属性值的引用（如果属性值是引用类型）。
- 对于原始对象的基本类型属性（如数字、字符串等），浅拷贝会直接复制其值给新对象的相应属性。
- 但是，对于原始对象的引用类型属性（如数组、对象等），浅拷贝只会复制属性的引用，而不会创建新的引用类型对象。
- const obj2 = {...info}、const obj3 = Object.assign({},info)



### 深拷贝

- 深拷贝是创建一个新对象，并且递归地复制原始对象的所有属性值，包括引用类型属性。

- 对于原始对象的基本类型属性，深拷贝会直接复制其值给新对象的相应属性，与浅拷贝相同。

- 但是，对于原始对象的引用类型属性，深拷贝会创建一个新的引用类型对象，并将原始对象的引用类型属性的值递归地复制到新的引用类型对象中。

- 因此，在深拷贝中，原始对象和新对象的引用类型属性指向不同的引用，修改其中一个对象的引用类型属性不会影响到另一个对象。



```javascript
function deepCopy(originValue) {
    if(!isObject(originValue)) {
        return originValue
    }
    
    const newObj = Array.isArray(originValue) ? [] : {}
    for (const key in originValue) {
        newObj[key] = deepCopy(originValue[key])
    }
    return newObj
}
```







## JSON

JSON：JavaScript object Notation（JavaScript对象符号）

其他传输格式：XML，Protobuf



### JSON值

1. 数字 (Number): 包括整数和浮点数。例如:
   { "age": 30 }
2. 字符串 (String): 字符串是由双引号包围的任意数量的 Unicode 字符。例如:
   { "name": "John Doe" }
3. 布尔值 (Boolean): 可以是 true 或 false。例如:
   { "isStudent": true }
4. 数组 (Array): 数组是值的有序集合。例如:
   { "scores": [90, 85, 95] }
5. 对象 (Object): 对象是一个无序的键值对集合。例如:
   { "person": { "name": "John", "age": 30 } }
6. 空值 (Null): 可以使用 null 表示空值或者不存在的值。例如:
   { "middleName": null }



### 相关方法

1. `JSON.stringify(~)`

   - 语法：JSON.stringify(value[, replacer[, space]])
     - `value`: 要转换为JSON字符串的JavaScript对象或值。
     - `replacer` (可选): 用于转换结果的函数或数组。如果是函数,则对象的每个属性都会经过该函数的转换和处理;如果是数组,则只有包含在这个数组中的属性名才会被序列化到最终的JSON字符串中。
     - `space` (可选): 指定缩进用的空白字符串,用于美化输出。如果参数是个数字,它代表有多少的空格;上限为10。该值若小于1,则意味着没有空格;如果该参数为字符串(字符串的前十个字母),该字符串将被作为空格。

   ```javascript
   const obj = {
     name: "John",
     age: 30,
     city: "New York",
     email: "john@example.com"
   };
   
   function replacer(key, value) {
     if (typeof value === "string") {
       return value.toUpperCase();
     }
     return value;
   }
   
   const jsonString = JSON.stringify(obj, replacer);
   console.log(jsonString);
   // 输出: {"name":"JOHN","age":30,"city":"NEW YORK","email":"JOHN@EXAMPLE.COM"}
   ```

   

2. `JSON.parse(~)`

   - `JSON.parse()`方法用于将一个JSON字符串解析为JavaScript对象。
   - 语法:JSON：parse(text[, reviver])
     - `text`: 要解析为JavaScript对象的JSON字符串。
     - `reviver` (可选): 一个转换结果的函数,将为对象的每个成员调用此函数。

   ```javascript
   const jsonString = '{"name":"John","age":30,"city":"New York"}';
   const obj = JSON.parse(jsonString);
   console.log(obj.name); // 输出: John
   console.log(obj.age); // 输出: 30
   ```




## 迭代器 iterator（了解）

使用户在`容器对象`上遍访的对象，使用该接口无需关心对象的内部实现细节。

迭代器通过定义 `next()` 方法来遍历数据，每次调用 `next()` 方法返回数据集合中的下一个元素。迭代器让我们能够通过循环的方式依次处理集合中的每个元素。

在JavaScript中，迭代器也是一个具体的对象，这个对象需要符合迭代器协议。

- 迭代器协议定义了<span style="color:red">产生一系列值的标准方式</span>
- 在JavaScript中这个标准就是一个<span style="color:red">特定的next方法</span>

- next方法：
  - 一个无参或者有参的函数，返回一个应当拥有以下两个属性的对象
  - done(boolean)
    - 如果迭代器可以产生序列中的下一个值，则为false。
    - 如果迭代器已将序列迭代完毕，则为true。
  - value



- 数组迭代器 简单实现

```javascript
const names = ["li", "wang", "zhang"]
function createArrayIterator(arr) {
    let index = 0
    return {
        next: function() {
            if(index < arr.length) {
                return {done: false, value: names[index++]}
            }else {
                return {done: true}
            }
        }
    }
}
```



### 可迭代对象

- 包括：数组、字符串、Map、Set等
- 可迭代对象必须实现了 `@@iterator` 方法（即 `Symbol.iterator` 属性）
- 对象中有一个属性，是一个函数，该函数含有返回迭代器，函数中next方法，返回done和value

#### 用于

- for...of语法、展开语法、yield、解构赋值

- 创建一些对象时：new Map([iterable])、new Set([iterable])
- 一些方法的调用：Promise.all(iterable)、Promise.race(iterable)、Array.from(iterable)





#### 把对象变成可迭代

```javascript
const obj = {
    name: 'li',
    age: 20,
    gender: 'female',

    [Symbol.iterator]: function () {
        const values = Object.keys(this)
        let index = 0
        const iterator = {
            next: function () {
                if (index < values.length) {
                    return { done: false, value: values[index++] }
                } else {
                    return { done: true }
                }
            }
        }
        return iterator
    }
}
for (let key of obj) {
    console.log(key)
} // name  age  gender
```



## 生成器 generator（了解）

- 生成器是ES6新增的一种函数控制、使用的方案，可以让我们更加灵活控制函数什么时候继续执行、暂停执行等。

- 生成器函数也是一个函数

  - 生成器函数需要在function后加 *

  - 生成器函数可以通过yield关键字控制函数的执行流程

  - 生成器函数的返回值是一个Generator（生成器）：

    生成器事实上是一种特殊的迭代器	

    

- 调用生成器函数，返回一个生成器对象
- 要想执行函数内部的代码，需要用返回的生成器对象，调用它的next操作

- 简单的例子

```javascript
function* foo(){
    console.log("1");
    yield 
    console.log("2");
    yield
    console.log("3");
}

const generator = foo();

generator.next(); // 1 {value: undefined, done:false}
generator.next(); // 2 {value: undefined, done:false}
generator.next(); // 3 {value: undefined, done:true}

function* foo1(){
    console.log("1");
    yield 'aaa'
}

const generator1 = foo1();
generator1.next(); // {value: 'aaa', done:false}
```



```javascript
function* foo1() {
    console.log("1");
    const textValue = yield 
    console.log('2',textValue);
}

const generator1 = foo1();
generator1.next()  // 1
generator1.next("hello")  // 2 hello  
```



### 提前结束

generator.return()

### 捕获异常

generator.throw(~)



### yield*

相当于yield语法糖，会依次迭代yield*之后的可迭代对象，每次迭代其中一个值

后面必须跟可迭代对象

```javascript
class Person {
    constructor(name, age, friends) {
        this.name = name
        this.age = age
        this.friends = friends
    }
    *[Symbol.iterator]() {
        yield* this.friends
    }
}

const p1 = new Person('Alice', 25, ['Bob', 'Charlie'])
for (const p of p1) {
    console.log(p);
}

const pIterator = p1[Symbol.iterator]()
console.log(pIterator.next());
console.log(pIterator.next());
console.log(pIterator.next());
```



```javascript
class Person {
    constructor(name, age, friends) {
        this.name = name
        this.age = age
        this.friends = friends
    }
    *[Symbol.iterator]() {
        yield* Object.values(this)  // 遍历value 
    }
}

const p1 = new Person('Alice', 25, ['Bob', 'Charlie'])
for (const p of p1) {
    console.log(p);
}
```



## 异常

throw可以抛出的类型：

- 基本数据类型：number、string、Boolean
- 对象类型

```javascript
function sum(num1, num2) {
	if(typeof num1 !== "number"){
        throw "type error：num1传入类型错误，必须为number类型"
    }
    if(typeof num2 !== "number"){
        throw "type error：num2传入类型错误，必须为number类型"
    }
    return num1 + num2
}
```



### Error类

错误函数的调用栈以及位置信息

- 属性：message，name，stack
- 子类：RangeError，SyntaxError，TypeError

```javascript
throw new Error("error")
```



### 捕获异常

try...catch...finally

ES10之后，catch后面的参数error可以省略

```javascript
try {
    foo()
}catch(error) {
    console.log(error)
}
```



## Storage存储

- localStorage：本地存储，提供的是一种<span style="color:red">永久的存储方法</span>，<span style="color:red">在关闭网页重新打开时，存储的内容依然保留</span>。
- sessionStorage：会话存储，提供的是<span style="color:red">本次会话的存储</span>，在关闭掉会话时，存储的内容会被清除



### localStorage

1. `setItem(key, value)`: 存储一个键值对。
   ```javascript
   localStorage.setItem('username', 'John');
   ```

2. `getItem(key)`: 根据键获取对应的值。如果键不存在，返回 null。
   ```javascript
   let username = localStorage.getItem('username');
   ```

3. `removeItem(key)`: 删除一个键值对。
   ```javascript
   localStorage.removeItem('username');
   ```

4. `clear()`: 删除所有的键值对。
   ```javascript
   localStorage.clear();
   ```

5. `key(index)`: 根据索引获取对应的键。
   ```javascript
   let key = localStorage.key(0);
   ```

6. `length`: 获取当前存储的键值对的数量。
   ```javascript
   let length = localStorage.length;
   ```



### sessionStorage

1. `setItem(key, value)`: 存储一个键值对。
   
   ```javascript
   sessionStorage.setItem('username', 'John');
   ```
   
2. `getItem(key)`: 根据键获取对应的值。如果键不存在，返回 null。
   ```javascript
   let username = sessionStorage.getItem('username');
   ```

3. `removeItem(key)`: 删除一个键值对。
   ```javascript
   sessionStorage.removeItem('username');
   ```

4. `clear()`: 删除所有的键值对。
   ```javascript
   sessionStorage.clear();
   ```

5. `key(index)`: 根据索引获取对应的键。
   ```javascript
   let key = sessionStorage.key(0);
   ```

6. `length`: 获取当前存储的键值对的数量。
   ```javascript
   let length = sessionStorage.length;
   ```



## es6+

### 相关名词

#### ECMA

ECMA国际 (Ecma international) 前身为 欧洲计算机制造商协会 ECMA（European Computer Manufacturers Association）

#### ECMAScript

由 ECMA国际 通过 ECMA-262 标准化的脚本设计语言





### es6简写语法

```javascript
const calculatro = {
    add: function (a, b) {
        return a + b
    },
    substract(a, b) {   // ES6简写语法
        return a - b
    }
}

console.log(calculatro.add(1, 2))
console.log(calculatro.substract(1, 2)) 
```

```javascript
const name = "小明";
const age = 25;

// 属性名与变量名相同时的简写
const user = {
  name,  // 相当于 name: name
  age,   // 相当于 age: age
  sayHi() {  // 方法简写
    return `我是${this.name}`;
  }
};
```



### es6模块化

在es6模块化诞生之前，javascript社区已经尝试并提出了AMD、CMD、CommonJS等模块化规范

- AMD和CMD适用于浏览器端的 javascript模块化
- CommonJS适用于服务器端的 Javascript模块化

#### `CommonJs规范`

node.js遵循了CommonJS的模块化规范，其中：

- 导入其他模块使用requier() 方法
- 模块对外共享成员使用module.exports对象



#### esmodule 

es6模块化规范

ES6模块化规范是浏览器端和服务器端通用的模块化开发规范

- 每个 js 文件都是一个独立的模块
- 导入其他模块成员使用 import 关键字
- 向外共享模块成员使用 export 关键字



##### 导出

**命名导出**

```javascript
// math.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

**默认导出**

```javascript
// config.js
const config = {
  apiKey: 'your-api-key',
  authDomain: 'your-auth-domain',
};

export default config;
```

##### 导入

**导入全部功能**：使用 `import * as` 关键字，可以导入模块中的所有功能并将它们放在一个对象中。

```javascript
// app.js
import * as math from './math.js';

console.log(math.add(2, 3)); // 使用命名导出的功能
console.log(math.subtract(5, 2));
```

**命名导入** ：使用 `import { functionName }` 语法，可以导入模块中的特定功能。

```javascript
// app.js
import { add, subtract } from './math.js';

console.log(add(2, 3)); // 使用命名导出的功能
console.log(subtract(5, 2));
```

也可以使用别名导入

```javascript
import { name as personName, sayHello as greet } from './module.js';
```

**默认导入**：使用 `import name from 'module'` 语法，可以导入默认导出的模块。你可以为导入的内容使用任何名称。

```javascript
// main.js
import config from './config.js';

console.log(config.apiKey); // 访问默认导出的内容
```



##### 严格模式：use strict

```javascript
//  加上
"use strict"
```



#### html文件采用esmodule加载js

使用`type="module"`

```html
<script scr="./foo.js" type="module"></script>
```







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

2. 对象的属性展开，可以用于克隆对象、合并对象（ES9）

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

### 数字位数语法糖

```javascript
if(count >10_0000_0000){}
// 和1000000000没有区别，便于阅读
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



### 箭头函数

- 箭头函数不会绑定this、arguments属性

- 箭头函数不会作为构造函数使用（不能和new一起使用）



- 省略

  - 如果函数只有一个参数，可以省略()

  - 如果函数体只有一行执行代码，可以省略{}，一行代码中不能带return关键字

    ```javascript
    var arrFn = () => 123
    ```

  - <span style="color:red">如果默认返回值是一个对象，且只有一行，那么这个对象必须加上()</span>

    ```javascript
    var arrFn1 = () => ({name: "li"})
    ```

    

### 对象属性简写

将一个变量作为对象的属性，并且属性名与变量名相同，可以使用简写语法

```javascript
let name = "John";
let age = 30;

let person = {
  name,
  age
};
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

 忽略某些值

```javascript
const [c,, d] = [3, 4, 5];，这里 c 的值为 3，d 的值为 5。
```

默认值

可以为变量设置默认值，以防对应位置的数组元素不存在或为 undefined

```javascript
const [e = 10, f = 20] = [6];，这里 e 的值为 6，f 的值为 20。
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



```javascript
const person = {
    name: 'John',
    age: 30,
    address: {
        street: '123 Main St',
        city: 'Anytown',
        state: 'CA'
    }
};

function deepTraverse(obj, parentKey = '') {
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            const fullKey = parentKey ? `${parentKey}.${key}` : key;

            if (typeof obj[key] === 'object' && obj[key] !== null) {
                // 如果值是对象，则递归遍历
                deepTraverse(obj[key], fullKey);
            } else {
                // 如果值是基本类型，则输出
                console.log(`${fullKey} -- ${obj[key]}`);
            }
        }
    }
}

deepTraverse(person);
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



### onclick和addEventListener

```javascript
const b1 = document.querySelector('button');

b1.addEventListener('click', function () {
    console.log(this);  // window对象
})

b1.onclick = function () {
    console.log(this);  // button
}
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



## arguments

```javascript
function showArgs() {
    console.log("参数个数:", arguments.length);
    console.log("第一个参数:", arguments[0]);
    console.log("所有参数:", Array.from(arguments));
}

showArgs("hello", 123, true);
// 输出:
// 参数个数: 3
// 第一个参数: hello
// 所有参数: ["hello", 123, true]
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

## API

### Notification 

`Notification` 是浏览器提供的一个构造函数，用于创建桌面通知。它是 HTML5 引入的一部分，可以在浏览器外层显示系统级通知提示。

使用 `Notification` 的流程一般是：

1. 请求用户授权
2. ****创建通知****

#### 示例

```javascript
// 第一步：请求用户授权
if (Notification.permission === 'granted') {
    // 已授权，可以发送通知
    new Notification('Hello!', {
        body: '这是通知的内容',
        icon: 'https://example.com/icon.png'
    });
} else if (Notification.permission !== 'denied') {
    // 请求授权
    Notification.requestPermission().then(permission => {
        if (permission === 'granted') {
            new Notification('Hello!', {
                body: '你刚刚允许了通知',
                icon: 'https://example.com/icon.png'
            });
        }
    });
}
```

#### 构造函数

```javascript
new Notification(title, options);
```

- `title`：通知的标题（必填）
- options（可选）：
  - `body`：通知的正文内容
  - `icon`：通知图标
  - `tag`：用于避免重复通知
  - `requireInteraction`：是否要求用户点击通知才消失
  - `silent`：是否静音通知

```javascript
new Notification("新消息", {
    body: "你有一条新消息来自 John。",
    icon: "https://example.com/message-icon.png"
});
```

#### 权限管理

浏览器有三种权限状态：

- `granted`：用户已授权
- `denied`：用户已拒绝
- `default`：用户还未作出选择

可以通过 `Notification.permission` 查看当前状态。



# 浏览器原理

## 浏览器如何渲染页面

当浏览器的网络线程收到HTML文档后，会产生一个渲染任务，并将其传递给渲染主线程的消息队列。

在事件循环机制的作用下，渲染主线程取出消息队列中的渲染任务，开启渲染流程

整个渲染流程分为多个阶段，分别是：HTML 解析、样式计算、布局、分层、绘制、分块、光栅化、画

每个阶段都有明确的输入输出，上一阶段的输出会成为下一个阶段的输入。

**渲染的第一步是解析HTML**

解析过程中遇到 CSS 解析CSS，遇到 JS 执行JS。为了提高解析效率，`浏览器在开始解析前，会启动一个预解析的线程`，率先下载HTML中外部CSS文件和外部的JS文件。

如果主线程解析到`link`位置，此时外部的CSS文件还没有下载解析好，主线程不会等待，继续解析后续的HTML。这是因为下载和解析CSS的工作室在预解析线程中进行的。这就是CSS不会阻塞HTML解析的根本原因。

如果主线程解析到`script`位置，会停止解析HTML，转而等待JS文件下载好，并将全局代码解析执行完成后，才能继续解析HTML。这是因为 JS 代码的执行过程可能会修改当前的 dom 树，所以dom树的生成必须暂停。这就是JS会阻塞HTML解析的根本原因。

第一步完成后，会得到 DOM 树和 CSSOM 树，浏览器的默认样式、内部样式、外部样式、行内样式均会包含在 CSSOM 树中。

**渲染的第二步是样式计算**

主线程会遍历得到的DOM树，依次为树中的每个节点计算出它最终的样式，称之为 Computed Style。

在这一过程中，很多预设值会变成绝对值，比如red会变成 rgb(255,0,0)相对单位会变成绝对单位，比如em会变成px

这一步完成后，会得到一棵带有样式的DOM树

**渲染的第三步是布局**

布局阶段会依次遍历DOM树的每一个节点，计算每个节点的几何信息。例如节点的宽高、相对包含块的位置。

大部分时候，DOM树和布局树并非一一对应。

比如`display:none`的节点没有几何信息，因此不会生成到布局树；又比如使用了伪元素选择器，虽然DOM树中不会存在这些伪元素节点，但它们拥有几何信息，所以会生成到布局树中。还有匿名行盒、匿名块盒等等都会导致DOM树和布局树无法一一对应。

**渲染的第四步是分层**

主线程会使用一套复杂的策略对整个布局树中进行分层。

分层的好处在于，将来某一个层改变后，仅会对该层进行后续处理，从而提高效率。

滚动条、堆叠上下文、transform、opacity 等样式都会或多或少的影响分层结果，也可以通过 `will-change`属性更大程度的影响分层结果。

**渲染的第五步是绘制**

 主线程会为每个层单独产生绘制指令集，用于描述这一层的内容该如何画出来

完成渲染后，渲染主线程将每个图层的绘制信息提交给合成线程，剩余工作将由合成线程完成。

合成线程首先对每个图层进行分块，将其划分为更多的小区域。

他会从线程池中拿取多个线程来完成分块工作。

**分块完成后，进入光栅化阶段**

合成线程会将块信息交给`GPU`,以极高的速度完成光栅化。

GPU进程会开启多个线程来完成光栅化，并且优先处理靠近视口区域的块。

光栅化的结果，就是一块一块的位图。

**最后一步就是画了**

合成线程拿到每个层、每个块的位图后，生成一个个指引（quad）信息。

指引会标识出每个位图应该画到屏幕的哪个位置，以及会考虑到旋转、缩放等变形。

变形发生在合成线程，与渲染主线程无关，这就是`transform`效率高的根本原因

合成线程会把quad提交给GPU进程，由GPU进程产生系统调用，提交给GPU硬件，完成最终的屏幕成像。





## 解析HTML

**styleSheets**

控制台输入

```
document.styleSheets[0].addRule('div','border:2px solid #f40 !important')
```

**HTML解析过程中遇到css**

为了提高解析效率，浏览器会启动一个预解析器率先下载和解析CSS



# 属性描述符

**Object.defineProperty** 

是vue2的响应式原理

**得到属性描述符**

```js
var obj ={
    a: 1,
    b: 2
}

var desc = Object.getOwnPropertyDescriptor(obj,'a')
//  { value: 1, writable: true, enumerable: true, configurable: true }
```



**设置属性描述符**

```js
Object.defineProperty(person, 'name', {
  value: 'John Doe',   // 属性的值
  writable: false,	// 是否可写
  enumerable: true,	// 是否可枚举
  configurable: false	// 是否可修改
});

// 定义一个具有getter和setter的属性
let internalValue = '';
Object.defineProperty(obj, 'accessor', {
  get: function() {
    return internalValue;
  },
  set: function(newValue) {
    internalValue = newValue;
  }
});
```



# 网络请求

- 早期的网页都是通过后端渲染来完成的：服务端渲染（SSR，server side render）
  - 服务器接收请求并返回相应的HTML文档
  - jsp、asp、php



- AJAX（Asynchronous JavaScript And Xml）：无页面刷新获取服务器数据













## HTTP

- `超文本传输协议`（HyperText Transfer Protocol）是一种用于分布式、协作式、超媒体信息系统的<span style="color:red">应用层协议</span>
- 通过HTTP，HTTPS协议请求的资源有`统一资源标识符`(Uniform Resource Identifiers，URI)来标识



- HTTP是一个客户端和服务端之间请求和响应的标准



### http版本

1. HTTP/0.9
   - 只支持get请求方法获取数据
2. HTTP/1.0
   - 支持POST、HEAD等请求方法，支持请求头、响应头等
   - 但是浏览器的每次请求都需要与服务器建立一个TCP连接，请求处理完后立即断开TCP连接
3. HTTP/1.1（使用最多）
   - 增加了PUT、DELETE等方法
   - 采用持久连接，多个请求可以共用同一个TCP连接
4. HTTP/2.0
5. HTTP/3.0



### 请求方式

1. GET：请求指定的资源。GET 请求应该只用于获取数据，而不应当产生其他影响。
2. POST：将实体提交到指定的资源，通常会导致在服务器上的状态变化或副作用。
3. PUT：用请求有效载荷替换目标资源的所有当前表现。
4. DELETE：删除指定的资源。
5. HEAD：与 GET 请求类似，但返回的响应中没有具体的内容，用于获取报头。
6. OPTIONS：用于描述目标资源的通信选项。
7. PATCH：用于对资源进行部分修改。
8. CONNECT：建立一个到由目标资源标识的服务器的隧道。通常用在代理服务器。
9. TRACE：沿着到目标资源的路径执行一个消息环回测试。



### HTTP Request

#### 请求行

由请求方法、请求的URI和HTTP协议版本组成

URL(Uniform Resource Locator**统一资源定位符**)



#### 请求头

1. <span style="color:red">Host</span>:指定请求的服务器域名和端口号。
2. <span style="color:red">User-Agent</span>:包含发出请求的用户信息,比如操作系统、浏览器类型及版本等。
3. <span style="color:red">Accept</span>:指定客户端可以接受的内容类型(MIME类型),如text/html, application/json等。
4. Accept-Encoding:指定客户端支持的压缩方式,如gzip, deflate等。
5. Accept-Language:指定客户端支持的语言,如en-US,zh-CN等。
6. Connection:设置持久连接,如keep-alive。
7. <span style="color:red">Cookie</span>:客户端发送给服务器的cookie信息。
8. <span style="color:red">Content-Type</span>:发送给服务器的请求体的MIME类型,常见的有:
   - application/json：用于指示请求或响应中的实体正文是 JSON 格式的数据。
   - application/x-www-form-urlencoded：用于指示请求中的实体正文是经过 URL 编码的表单数据，在 HTML 表单中常用的形式。
   - multipart/form-data：用于指示请求中的实体正文是多部分表单数据，常用于文件上传等场景。
   - text/plain：用于指示请求或响应中的实体正文是纯文本数据，没有特定的格式或结构。
   - image/jpeg、image/png、image/gif 等：用于指示实体正文是相应格式的图像数据。
   - application/xml：用于指示请求或响应中的实体正文是 XML 格式的数据。
   - application/pdf：用于指示实体正文是 PDF 格式的数据。
   
9. Content-Length:请求体的字节长度。
10. Authorization:身份验证信息,常用于Basic Auth和OAuth。
11. Origin:发起请求的页面的源地址(协议+域名+端口),用于CORS跨域请求。
12. Referer:表示请求的来源页面的URL,可以用于防盗链和统计分析。
13. If-Modified-Since/If-None-Match:用于缓存控制,如果本地资源未过期就从缓存中获取。
14. X-Requested-With:异步请求标识,常见的如XMLHttpRequest。
15. DNT(Do Not Track):表示用户不希望被追踪,1表示启用,0表示禁用。
16. Upgrade-Insecure-Requests:表示客户端优先选择HTTPS。
17. Pragma:HTTP/1.0时代用于控制缓存的字段,常见的如no-cache。
18. Cache-Control:控制缓存的字段,常见的如no-cache, no-store, max-age等。



#### 空行

用于分隔请求头和请求体

#### 请求体



## XHR

`XMLHttpRequest`（简称XHR）是浏览器提供的 JavaScript对象，通过它，可以请求服务器上的数据资源，从URL获取数据而无需让整个页面刷新。Ajax函数，就是基于xhr对象封装出来了的。它可以发送和接收各种格式的数据，包括 JSON、XML、HTML 和纯文本。



## 区别一般http请求和ajax请求

1. ajax请求是一种特别的http请求

2. 对服务器来说没有任何区别，区别在浏览器端

3. 浏览器端发请求：只有`XHR`或`fetch`发送的才是ajax请求。

   **Ajax** 是 **Asynchronous JavaScript and XML** 的简称，其本质是一个概念或技术组合，而不是特定的工具或 API。

   **Fetch API** 也能实现异步 HTTP 请求，但是传统意义上的 Ajax 通常指的是通过 **XHR** 实现的异步请求，而 Fetch 是一种全新的方式，它摆脱了对 XHR 的依赖，拥有更现代的设计。

4. 浏览器端接收到响应

   - 一般请求：浏览器会直接显示响应体数据，也就是我们常说的刷新/跳转页面
   - ajax请求：浏览器不会对界面进行任何更新操作，只是调用监视的回调函数并传入相应相关数据。

## axios

发送ajax请求：`XHR对象`和`fetch函数`

可以用json-server 搭建rest接口 

```node
npm i -g json-server
```



### html使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.7.2/axios.js"></script>
</head>

<body>
    <div><button class="d1" onclick="testHandle()">点击</button></div>
</body>
<script>
    function testHandle() {
        axios.post("http://ld.zjldsoft.com/ld/common/v1/materialpharmsort?method=getPharmSort", { "kf_id": "16" })
            .then(response => {
                console.log(response.data)
            }).catch(error => alert(error))
    }
</script>

</html>
```



## 赋值表达式

在 JavaScript 中，赋值表达式（`=`）的返回值是被赋的值。

```javascript
let a;
console.log(a = 42); // 输出: 42

const obj = {};
console.log(obj["key"] = 123); // 输出: 123
```



# 其他

## 策略模式代替 if-else

```javascript
function getPermission(role) {
    if (role === 'admin') {
        return 'Full Access';
    } else if (role === 'editor') {
        return 'Edit Content';
    } else if (role === 'viewer') {
        return 'View Content';
    } else {
        return 'No Access';
    }
}

console.log(getPermission('admin')); // Full Access
console.log(getPermission('viewer')); // View Content
```

策略模式

```javascript
const permission = {
    admin: () => 'Full Access',
    editor: () => 'Edit Content',
    viewer: () => 'View Content',
    default: () => 'No Access'
}

function getPermission(role) {
    return (permission[role] || permission["default"])();
}

console.log(getPermission('admin')); // Full Access
console.log(getPermission('editor')); // Edit Content
console.log(getPermission('viewer')); // View Content
console.log(getPermission('unknown')); // No Access
```





