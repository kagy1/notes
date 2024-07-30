# Vue.js设计与实现笔记

## 第⼀篇 框架设计概览

### 第 1 章：框架设计概览

- 命令式

早年间流⾏的 jQuery 就是典型的命令式框架

命令式框架的⼀⼤特点就是 <span style="color:red">关注过程</span>

```js
01 - 获取 id 为 app 的 div 标签
02 - 它的⽂本内容为 hello world
03 - 为其绑定点击事件
04 - 当点击时弹出提⽰：ok

01 $('#app') // 获取 div
02 .text('hello world') // 设置⽂本内容
03 .on('click', () => { alert('ok') }) // 绑定点击事件

01 const div = document.querySelector('#app') // 获取 div
02 div.innerText = 'hello world' // 设置⽂本内容
03 div.addEventListener('click', () => { alert('ok') }) // 绑定点击事件
```

- 声明式

声明式框架更加关注结果

```js
01 <div @click="() => alert('ok')">hello world</div>
```



Vue.js 的内 部实现⼀定是命令式的，⽽暴露给⽤户的却更加声明式



如果我们把直接修改的性能消耗定义为 A，把找出差异的性能消 耗定义为 B，

那么有： 命令式代码的更新性能消耗 = A 

​				声明式代码的更新性能消耗 = B + A

`声明式代码的更新性能消耗 = 找出差异的性能消耗 + 直接修改的性能消耗`

<span style="color:red">因此，如果我们能够最⼩化找出差异的性能消 耗，就可以让声明式代码的性能⽆限接近命令式代码的性能。⽽所谓 的虚拟 DOM，就是为了最⼩化找出差异这⼀步的性能消耗⽽出现的。</span>



虚拟 DOM 创建⻚⾯的过程分为两步：

1. 第⼀步是创建 JavaScript 对象(VNode)，这个对象 可以理解为真实 DOM 的描述；

2. 第⼆步是递归地遍历虚拟 DOM 树并 创建真实 DOM。

我们同样可以⽤⼀个公式来表达：<span style="color:red">创建 JavaScript 对象的计算量 + 创建真实 DOM 的计算量。</span>



虚拟 DOM更新⻚⾯:<span style="color:red">它需要重新创 建 JavaScript 对象（虚拟 DOM 树），然后⽐较新旧虚拟 DOM(diff)，找到变化的元素并更新它。</span>

虚拟 DOM 在更新⻚⾯时只会更新必要的元 素，但 innerHTML 需要全量更新。

<img src=".\img\1.jpg" align="left" style="zoom:80%;" />



Vue.js 3 仍然保持了运 ⾏时 + 编译时的架构。分析⽤户提供的内容，看看哪些内容未来可能会改变， 哪些内容永远不会改变，这样我们就可以在编译的时候提取这些信息，然后将其传递给 Render 函数

<span style="color:red">声明 式的更新性能消耗 = 找出差异的性能消耗 + 直接修改的性能消耗</span>



### 第 2 章：框架设计的核⼼要素

热更新（hot module replacement，HMR）

- rollup.js

Vue.js 使⽤ rollup.js 对项⽬进⾏构建

```node.js
npm install --global rollup  //这将使 Rollup 可以作为全局命令行工具使用
```

Rollup 是一个用于 JavaScript 的模块打包工具，它将小的代码片段编译成更大、更复杂的代码，例如库或应用程序。它使用 JavaScript 的 ES6 版本中包含的新标准化代码模块格式，而不是以前的 CommonJS 和 AMD 等特殊解决方案。ES 模块允许你自由无缝地组合你最喜欢的库中最有用的个别函数。这在未来将在所有场景原生支持，但 Rollup 让你今天就可以开始这样做。



Tree-Shaking：消除那些永远不会被执⾏的 代码，也就是排除 dead code

​							想要实现 Tree-Shaking，必须满⾜⼀个条件，即模块必须是 ESM（ES Module）

注释代码` /*#__PURE__*/`，其作⽤就是告诉 rollup.js，对 于 foo 函数的调⽤不会产⽣副作⽤，你可以放⼼地对其进⾏ Tree- Shaking

<span style="color:red;">IIFE : Immediately Invoked Function Expression</span>，即“立即调⽤的函数表达式”

```js
(function () {
// ...
}())
```



### 第 3 章 Vue.js 3 的设计思路

虚拟DOM（virtual DOM）简称vdom，是一个普通的[js对象](https://so.csdn.net/so/search?q=js对象&spm=1001.2101.3001.7020)，用来描述真实dom结构，因为它不是真实的DOM，所以称为虚拟DOM



渲染器的作⽤就是<span style="color:red">把虚拟 DOM 渲染为真实 DOM</span>

<b style="color:red;">组件 就是⼀组 DOM 元素的封装</b>，这组 DOM 元素就是组件要渲染的内 容，因此我们可以定义⼀个函数来代表组件，⽽函数的返回值就代表 组件要渲染的内容



编译器的作⽤其实就是<span style="color:red">将模板编译为渲染函数</span>

```tsx
<div @click="handler">
click me
</div>

//编译为
render() {
return h('div', { onClick: handler }, 'click me')
}
```

组件的实现依赖于`渲染器`，模板的编译依赖于`编译器`

编译器能识别出哪些是静态属性，哪些是动态属性，在⽣成代码的时候 完全可以附带这些信息

渲染器的作⽤是，把虚拟 DOM 对象渲染为真实 DOM 元素。它的⼯作原理是，递归地遍历虚拟 DOM 对象，并调⽤原⽣ DOM API 来完成真实 DOM 的创建。 渲染器的精髓在于后续的更新，它会通过 Diff 算法找出变更点，并且只会更新需要更新的内容。

## 第⼆篇 响应系统

### 第 4 章 响应系统的作⽤与实现

副作⽤函数指的是会产⽣副作⽤的函数

如

```js
function effect() {
document.body.innerText = 'hello vue3'
}
```

当 effect 函数执⾏时，它会设置 body 的⽂本内容，但除了 effect 函数之外的任何函数都可以读取或设置 body 的⽂本内容。 也就是说，effect 函数的执⾏会直接或间接影响其他函数的执⾏， 这时我们说 effect 函数产⽣了副作⽤。



- 拦截⼀个对象属性的读取和 设置操作

  在 ES2015 之前，只能通过 Object.defineProperty 函数实现，这也是 Vue.js 2 所采⽤的⽅式。

​		在 ES2015+ 中，我们可 以使⽤代理对象 Proxy 来实现，这也是 Vue.js 3 所采⽤的⽅式。











### 第 5 章 ⾮原始值的响应式⽅案



### 第 6 章 原始值的响应式⽅案
