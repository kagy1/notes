# Vite

## webpack

一般的项目使用 Webpack 之后，启动花个几分钟都是很常见的事情，热更新也经常需要等待十秒以上。这主要是因为：

-   项目冷启动时必须递归打包整个项目的依赖树
-   JavaScript 语言本身的性能限制，导致构建性能遇到瓶颈，直接影响开发效率

这样一来，代码改动后不能立马看到效果，自然开发体验也越来越差。而其中，最占用时间的就是代码打包和文件编译。

### Babel

Babel是一个工具链，主要用于旧浏览器或者环境中将ECMAScript 2015+(es6+)代码转换为向后兼容版本的JavaScript

包括：语法转换、源代码转换等

#### Babel命令行使用

作为独立工具，不和webpack一起使用

- @babel/core：babel核心代码，必须安装
- @babel/cli：命令行使用babel

```
npm install @babel/core -g
npm install @babel/cli -g
```



#### Babl预设Preset

```
npm install @babel/preset-env -g
```

```
npx babel demo.js --out-file test.js --presets=@babel/preset-env
```



#### use strict

`"use strict";` 是 JavaScript 中的一种严格模式声明，它会让代码运行在严格模式下。严格模式是 ES5 引入的一种更严格的运行环境，它修复了一些历史遗留问题，并引入了一些限制以帮助开发者编写更安全、规范的代码。





## 模块标准

### 无模块化标准阶段

#### 1. 文件划分

#### 2. 命名空间

`命名空间`是模块化的另一种实现手段，它可以解决上述文件划分方式中`全局变量定义`所带来的一系列问题。下面是一个简单的例子:

```ts
// module-a.js
window.moduleA = {
  data: "moduleA",
  method: function () {
    console.log("execute A's method");
  },
};
```

```ts
// module-b.js
window.moduleB = {
  data: "moduleB",
  method: function () {
    console.log("execute B's method");
  },
};
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <script src="./module-a.js"></script>
    <script src="./module-b.js"></script>
    <script>
      // 此时 window 上已经绑定了 moduleA 和 moduleB
      console.log(moduleA.data);
      moduleB.method();
    </script>
  </body>
</html>
```

这样一来，每个变量都有自己专属的命名空间，我们可以清楚地知道某个变量到底属于哪个`模块`，同时也避免全局变量命名的问题。

#### 3. IIFE(立即执行函数)

不过，相比于`命名空间`的模块化手段，`IIFE`实现的模块化安全性要更高，对于模块作用域的区分更加彻底。你可以参考如下`IIFE 实现模块化`的例子:

```ts
// module-a.js
(function () {
  let data = "moduleA";

  function method() {
    console.log(data + "execute");
  }

  window.moduleA = {
    method: method,
  };
})();
```

```ts
// module-b.js
(function () {
  let data = "moduleB";

  function method() {
    console.log(data + "execute");
  }

  window.moduleB = {
    method: method,
  };
})();
```

```html
// index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <script src="./module-a.js"></script>
    <script src="./module-b.js"></script>
    <script>
      // 此时 window 上已经绑定了 moduleA 和 moduleB
      console.log(moduleA.data);
      moduleB.method();
    </script>
  </body>
</html>
```

我们知道，每个`IIFE` 即`立即执行函数`都会创建一个私有的作用域，在私有作用域中的变量外界是无法访问的，只有模块内部的方法才能访问。拿上述的`module-a`来说:

```ts
// module-a.js
(function () {
  let data = "moduleA";

  function method() {
    console.log(data + "execute");
  }

  window.moduleA = {
    method: method,
  };
})();
```

对于其中的 `data`变量，我们只能在模块内部的 `method` 函数中通过闭包访问，而在其它模块中无法直接访问。这就是模块`私有成员`功能，避免模块私有成员被其他模块非法篡改，相比于`命名空间`的实现方式更加安全。

但实际上，无论是`命名空间`还是`IIFE`，都是为了解决全局变量所带来的命名冲突及作用域不明确的问题，也就是在`文件划分方式`中所总结的`问题 1` 和`问题 2`，而并没有真正解决另外一个问题——**模块加载**。如果模块间存在依赖关系，那么 script 标签的加载顺序就需要受到严格的控制，一旦顺序不对，则很有可能产生运行时 Bug。



#### CommonJS 规范

```ts
// module-a.js
var data = "hello world";
function getData() {
  return data;
}
module.exports = {
  getData,
};

// index.js
const { getData } = require("./module-a.js");
console.log(getData());
```



#### AMD规范



#### ES6 Module

`ES6 Module` 也被称作 `ES Module`(或 `ESM`)， 是由 ECMAScript 官方提出的模块化规范

在现代浏览器中，如果在 HTML 中加入含有`type="module"`属性的 script 标签，那么浏览器会按照 ES Module 规范来进行依赖加载和模块解析

下面是一个使用 ES Module 的简单例子:

```ts
// main.js
import { methodA } from "./module-a.js";
methodA();

//module-a.js
const methodA = () => {
  console.log("a");
};

export { methodA };
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/src/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="./main.js"></script>
  </body>
</html>
```

如果在 Node.js 环境中，你可以在`package.json`中声明`type: "module"`属性:

```ts
// package.json
{
  "type": "module"
}
```

然后 Node.js 便会默认以 ES Module 规范去解析模块:

```ts
node main.js
// 打印 a
```





## 样式方案

1. `CSS 预处理器`：主流的包括`Sass/Scss`、`Less`和`Stylus`。这些方案各自定义了一套语法，让 CSS 也能使用嵌套规则，甚至能像编程语言一样定义变量、写条件判断和循环语句，大大增强了样式语言的灵活性，解决原生 CSS 的**开发体验问题**。
2. `CSS Modules`：能将 CSS 类名处理成哈希值，这样就可以避免同名的情况下**样式污染**的问题。
3. CSS 后处理器`PostCSS`，用来解析和处理 CSS 代码，可以实现的功能非常丰富，比如将 `px` 转换为 `rem`、根据目标浏览器情况自动加上类似于`--moz--`、`-o-`的属性前缀等等。
4. `CSS in JS` 方案，主流的包括`emotion`、`styled-components`等等，顾名思义，这类方案可以实现直接在 JS 中写样式代码，基本包含`CSS 预处理器`和 `CSS Modules` 的各项优点，非常灵活，解决了开发体验和全局样式污染的问题。
5. CSS 原子化框架，如`Tailwind CSS`、`Windi CSS`，通过类名来指定样式，大大简化了样式写法，提高了样式开发的效率，主要解决了原生 CSS **开发体验**的问题。

### CSS 预处理器

Vite 本身对 CSS 各种预处理器语言(`Sass/Scss`、`Less`和`Stylus`)做了内置支持。也就是说，即使你不经过任何的配置也可以直接使用各种 CSS 预处理器。

### CSS Modules

CSS Modules 在 Vite 也是一个开箱即用的能力，Vite 会对后缀带有`.module`的样式文件自动应用 CSS Modules。



## 创建vue3项目

```node.js
npm create vue@latest

npm init @vitejs/app  // 允许你创建各种类型的项目,不仅限于 Vue。
```



## 本地服务器Connect



## ESBuild

### 特点

- 支持ES6和CommonJs模块化
- 支持ES6的Tree Shaking
- 支持Typescript，JSX等语法翻译





## 处理静态资源

### 图片加载

#### 使用场景

1. 在 HTML 或者 JSX 中，通过 img 标签来加载图片，如:

```html
<img src="../../assets/a.png"></img>
```

2. 在 CSS 中通过 background 属性加载图片，如:

```css
background: url('../../assets/b.png') norepeat;
```

3. 在 JavaScript 中，通过脚本的方式动态指定图片的`src`属性，如:

```ts
document.getElementById('hero-img').src = '../../assets/c.png'
```



#### 在 Vite 中使用

##### 配置别名

```ts
// vite.config.ts
import path from 'path';

{
  resolve: {
    // 别名配置
    alias: {
      '@assets': path.join(__dirname, 'src/assets')
    }
  }
}
```



## Rollup

Rollup 是一款基于 ES Module 模块规范实现的 JavaScript 打包工具



### Tree Shaking

可以分析出未使用到的模块并自动擦除





## 配置文件

项目中一般使用`vite.config.ts`作为配置文件

### 别名配置

```ts
// vite.config.ts
import path from 'path';

{
  resolve: {
    // 别名配置
    alias: {
      '@assets': path.join(__dirname, 'src/assets'),
      '~': resolve(process.cwd()),
      '@/': `${resolve(__dirname, 'src')}/`
    }
  }
}
```





## 环境变量

### import.meta.env

正式环境不存在

- import.meta.env.MODE	应用运行的模式
- import.meta.env.BASE_URL    部署应用时的基本 URL。他由`base` 配置项决定。 
- import.meta.env.PROD    应用是否运行在生产环境。
- import.meta.env.DEV    应用是否运行在开发环境。
- import.meta.env.SSR    应用是否运行在 server 上。



### 自定义环境变量

在项目的根目录下，新建一个` .env` 文件。 加载的环境变量也会通过 import.meta.env 以字符串形式暴露给客户端源码。 为了防止意外地将一些环境变量泄漏到客户端，<span style="color:red">Vite规定：只有以 VITE_ 为前缀的变量才会暴露给经过 vite 处理的代码。</span>

- `.env`文件内容

```
VITE_MY_KEY= 123
```

此时，我们再打印下`import.meta.env` 对象，就会出现`VITE_MY_KEY`变量

```
BASE_URL: "/"
DEV: true
MODE: "development"
PROD: false
SSR: false
VITE_MY_KEY: "hello world"
```



vite内置的环境变量属性是有代码智能提示补全的，如果你想要自己自定义的环境变量也有智能提示补全，你可以在 src 目录下创建一个 `env.d.ts` 文件，接着按下面这样增加 ImportMetaEnv 的定义：

```
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_MY_KEY: string
  // 更多环境变量...
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```



在真正项目的开发中，我们并不会直接去用`.env`文件，而是会新建`.env.development`文件和`.env.production`文件：

- `.env.development`文件：开发环境下会读取文件里面定义的数据
- `.env.production`文件：生产环境下会读取文件里面定义的数据

```
# .env.development
VITE_TITLE=开发环境的标题
# .env.production
VITE_TITLE=生产环境的标题
```

然后我们可以使用 import.meta.env.VITE_TITLE，在不同的环境下渲染不同的值



### 模式

<span style="color:red">在 Vite 中，可以通过 `--mode` 参数来指定不同的运行模式</span>

我们现在在项目的根目录下，新建一个` .env.test` 文件，对应test模式，内容如下

```
# .env.test
VITE_TITLE=test
```

在` package.json`里 新增配置

```
{
  "scripts": {
    "dev": "vite --mode test",
    "build": "run-p type-check \"build-only {@}\" --",
    "preview": "vite preview",
    "test:unit": "vitest",
    "build-only": "vite build",
    "type-check": "vue-tsc --build --force"
  }
}
```





## env文件

```
.env                # 所有情况下都会加载
.env.[mode]         # 只在指定模式下加载

.env                
.env.development
.env.production
.env.test
```



- .env文件

```
# 所有环境都会加载
# title
VITE_GLOB_APP_TITLE = YingSide_DEMO
 
# 本地运行端口号
VITE_PORT = 3000
 
# 启动时自动打开浏览器
VITE_OPEN = true
```



- .env.development文件

```
# 只在开发环境加载
VITE_USER_NODE_ENV = development
 
# 打包时是否删除 console
VITE_DROP_CONSOLE = true
 
# 公共基础路径
VITE_PUBLIC_PATH = /
 
# 开发环境接口地址
VITE_API_URL = /api
 
# 开发环境跨域代理，可配置多个
VITE_PROXY = [["/api","https://mock.mengxuegu.com/mock/6453b964af3bc37f99a23916"]]
```



- .env.production文件

```
# 只在生产环境加载
VITE_USER_NODE_ENV = production
 
# 公共基础路径
VITE_PUBLIC_PATH = /
 
# 是否启用 gzip 或 brotli 压缩打包，如果需要多个压缩规则，可以使用 “,” 分隔
# Optional: gzip | brotli | none
VITE_BUILD_COMPRESS = none
 
# 打包压缩后是否删除源文件
VITE_BUILD_COMPRESS_DELETE_ORIGIN_FILE = false
 
# 打包时是否删除 console
VITE_DROP_CONSOLE = true
 
# 是否启用图片压缩
VITE_USE_IMAGEMIN = true
 
# 线上环境接口地址
VITE_API_URL = "http://www.yanhongzhi.com"
```



- .env.test文件

```
# 测试环境
VITE_USER_NODE_ENV = test
 
# 公共基础路径
VITE_PUBLIC_PATH = /
 
# 是否启用 gzip 或 brotli 压缩打包，如果需要多个压缩规则，可以使用 “,” 分隔
# Optional: gzip | brotli | none
VITE_BUILD_COMPRESS = none
 
# 打包压缩后是否删除源文件
VITE_BUILD_COMPRESS_DELETE_ORIGIN_FILE = false
 
# 打包时是否删除 console
VITE_DROP_CONSOLE = true
 
# 测试环境接口地址
VITE_API_URL = "http://www.yanhongzhi.com"
```



### 确定当前环境

在`package.json`文件内

通过 --mode 配置

```
"scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "build:dev": "vue-tsc && vite build --mode development",   
    "build:test": "vue-tsc && vite build --mode test",
    //....
}
```



```
"build": "vue-tsc && vite build"
// vue-tsc 和 vite build 两个命令一起使用
// vue-tsc 是 Vue 3 项目中的一个命令，用于执行 TypeScript 的类型检查
// vite build 是 Vite 构建工具的构建命令，用于将项目代码打包和优化，生成可部署的生产版本
// vite 命令是 Vite 构建工具提供的一个开发服务器命令，用于在开发过程中启动一个本地开发服务器，并实时加载和更新代码
```





## HMR热更新

HMR 的全称叫做`Hot Module Replacement`，即`模块热替换`或者`模块热更新`。

### 模块更新时逻辑: hot.accept

在 `import.meta.hot` 对象上有一个非常关键的方法`accept`，因为它决定了 Vite 进行热更新的边界

`import.meta.hot.accept` 是用来手动处理模块热替换的 API。

- 在 Vue 3 + Vite 项目中，Vite 的 HMR 机制是自动处理的，Vue 官方插件（`@vitejs/plugin-vue`）已经帮你实现了 HMR 的逻辑。

  因此，**普通的 Vue 项目中通常不需要手动调用 `import.meta.hot.accept`**，所以你在全局代码中找不到它。

从字面上来看，它表示接受的意思。没错，它就是用来**接受模块更新**的。 一旦 Vite 接受了这个更新，当前模块就会被认为是 HMR 的边界。那么，Vite 接受谁的更新呢？这里会有三种情况：

-   接受**自身模块**的更新
-   接受**某个子模块**的更新
-   接受**多个子模块**的更新



## glob-import批量导入

### import.meta.glob

**懒加载（Lazy Loading）**：

- 默认返回一个对象，其中每个键是匹配到的文件路径，值是一个函数。
- 模块不会立即加载，只有在你手动调用这些函数时，才会动态加载对应的模块。

```javascript
const modules = import.meta.glob(pattern, options);
```

**`pattern`** (必填)：指定匹配文件的路径和规则。

- 使用类似 `glob` 的路径匹配模式（如 `*.js`、`**/*.vue`）。
- 支持绝对路径和相对路径。

**`options`** (可选)：配置选项对象。

- `eager`：是否立即加载模块（默认为 `false`）。
- `import`：自定义导入的模块（例如只导入模块的默认导出）。
- `query`：附加的查询参数。

示例

```javascript
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        const modules = import.meta.glob('@/components/glob/**/*.js')
        console.log(modules)
// 输出类似于：
// {
//   '/src/components/glob/1.js': () => import('./src/components/glob/1.js'),
//   '/src/components/glob/2.js': () => import('./src/components/glob/2.js')
// }
        console.log(modules['/src/components/glob/1.js'])
        const Component = ref(null)

        onMounted(async () => {
            const module: any = await modules['/src/components/glob/1.js']()
            Component.value = module.default
        })

        return () => (
            <div>

            </div>
        )
    }
})

```



### import.meta.globEager

<span style="color: red; font-weight: bold;">升级vite5后globEager这个方法就已经被弃用了</span>

原来的 const modules = import.meta.globEager('./modules/**/*.ts')  这种已经失效，

必须修改为 

const modules = import.meta.glob('./modules/**/*.ts', { eager: true, import: 'default' })

**立即加载（Eager Loading）**：

- 返回一个对象，其中每个键是匹配到的文件路径，值是已经加载的模块内容。
- 模块会在调用 `import.meta.globEager` 时立即加载，而不是等到手动调用时再加载。

```tsx
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        const modules = import.meta.glob('@/components/glob/**/*.js', { eager: true })
        console.log(Object.keys(modules))

        return () => (
            <div>
            </div>
        )
    }
})
```







## vite.config.js

### 修改端口

```
 server: {
    port: 3000
 }
```



### 配置别名

```
resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
}
```



### 其他配置

```
server: {
      hmr: true,   // 热模块替换
      cors: true,  // 跨域资源共享
      host: true,  // 允许通过 IP 访问开发服务器
      port: viteEnv.VITE_BASE_PORT  // 服务器端口
} 
```



# 