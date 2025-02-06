# Vite

## 构建工具功能：

1. 直接从node_modules里引入代码 + 多种模块化支持

2. 处理代码兼容性：babel降级，less，ts语法转换（构建工具将这些语法对应的处理工具集成进来自动化处理）

3. 提高项目性能：压缩文件，代码分割

4. 优化开发体验：

   - 构建工具会自动监听文件变化，文件变化以后会自动调用对应集成工具进行重新打包，在浏览器重新运行（热更新）

   - 开发服务器：解决跨域问题



构建工具让我们不用每次都关心代码在浏览器如何运行，只需要首次给构建工具一个配置文件，在下次需要更新时调用一次对应命令。

他让我们不用关心生产的代码也不用关心代码如何在浏览器里运行。





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



## vite解决webpack的问题

因为webpack支持多种模块化，一开始必须要统一模块化代码，意味着需要把所有的依赖读一遍

vite是基于es modules的，vite先启动开发服务器，当浏览器访问某个页面时，Vite 会根据页面中的 `import` 语句，按需加载依赖模块。

如果是非 ESM 格式的依赖库（如 CommonJS 模块），Vite 会通过 `esbuild` 将它们转化成 ESM 格式



## vite和vite脚手架的区别

安装vite脚手架

```
npm create vite 
npm create vite@latest
```

可以通过附加的命令行选项直接指定项目名称和你想要使用的模板

```
# npm 7+，需要添加额外的 --：
npm create vite@latest my-vue-app -- --template vue
```



create vite和vite的关系：create vite内置了vite

vue cli内置了webpack



## vite初体验

开箱即用(out of box)：不需要做任何额外的配置就可以使用vite来帮你处理构建工作



## vite预加载 / 依赖预构建

找寻依赖的过程是自当前目录向上查找的过程，知道搜寻到根目录或者搜寻到对应的依赖为止

1. **CommonJS 和 UMD 兼容性：**在开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将以 CommonJS 或 UMD 形式提供的依赖项转换为 ES 模块

2. **性能：** 为了提高后续页面的加载性能，Vite 将那些具有许多内部模块的 ESM 依赖项转换为单个模块。

   有些包将它们的 ES 模块构建为许多单独的文件，彼此导入。例如，lodash-es 有超过 600 个内置模块！当我们执行 `import { debounce } from 'lodash-es' `时，浏览器同时发出 600 多个 HTTP 请求！即使服务器能够轻松处理它们，但大量请求会导致浏览器端的网络拥塞，使页面加载变得明显缓慢。

   通过将 lodash-es 预构建成单个模块，现在我们只需要一个HTTP请求！







生产环境：vite会全权交给`rollup`完成生成环境的打包

依赖预构建：首先vite会找到对应的依赖，然后调用`esbuild`（对js语法进行处理）将其他规范的代码转换为esmodule规范，然后放到当前目录下的`node_moudules/.vite/deps`，同时对esmodule规范进行统一集成

解决了三个问题：

1. 不同的第三方包会有不同的导出格式（commonjs,esmodule等），通过esbuild处理
2. 对路径上的处理可以直接使用.vite/deps，方便路径重写
3. 网络多包传输的性能问题





Koa：node端的一个框架





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



## vite处理css

 vite天生支持对css文件直接处理

1. vite读取到main.js里引用了index.css
2. 直接使用fs模块读取index.css文件内容
3. 直接创建一个style标签，将index.css文件内容复制进style标签里
4. 将该css文件中的内容直接替换为js脚本（方便热更新，css模块化），同时设置Content-Type为js，让浏览器以JS脚本的形式执行该css文件



### vite.config.ts配置css

#### modules

```typescript
export default defineConfig {
    css: {
        modules: {
            localsConvention: "camelCase"，  // 配置类名转换，生成的配置对象的key的展现形式，驼峰还是中划线
            // localsConvention?: 'camelCase' | 'camelCaseOnly' | 'dashes' | 'dashesOnly'
            scopeBehaviour: "local"，  // 配置当前的模块化行为是模块化还是全局化
            // "local" | "global"
            generateScopedName: '[name]__[local]--[hash:base64:5]'  // 类名格式，可以配置为函数
            hashPrefix: "hello" // 生成hash的时候会根据类名生成，如果要生成的hash更加独特，可以配置hashPrefix
            globalModulePaths:["./indexA.module.css"] // 不想参与css模块化的路径
        }
    }
}
```

#### preprocessorOptions

```typescript
export default defineConfig {
    css: {
        preprocessorOptions: {
            less: {
                math: "always"
                globalVars: {  // 全局变量
                    mainColor: "red"，
                    devSourcemap: true  // 开启css的sourceMap（文件索引）
                }
            }
        }
    }
}
```



#### less

```
yarn add less // lessc的编译器
```

主要安装了less 就可以使用lessc去编译less文件

```
npx lessc --math="always" index.module.less
```



## PostCss

PostCSS 是一个使用 `JavaScript 插件转换 CSS` 的工具。

  可以对高级css语法降级，前缀补全

postcss的less，sass插件已经停止维护。要用less，Sass编译完成后把结果给postcss





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





## 配置文件 vite.config.ts

项目中一般使用`vite.config.ts`作为配置文件

为什么`vite.config.ts`可以书写esmodule形式，因为vite他在读取`vite.config.ts`的时候回率先去解析文件语法，如果是esmodule规范会换成commonjs规范

### defineConfig 

使用`defineConfig`配置，有属性提示

```typescript
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    vueJsx(),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  server: {
    port: 3001
  }
})

```

defineConfig() 工具函数默认支持 ts 的类型提示。
defineConfig() 可以接收一个配置对象`{}`为参数，也可以接收一个函数`()=>{}`为参数；当接收一个函数为参数时，函数的参数为一个条件对象。

Vite 配置文件（`vite.config.ts` 或 `vite.config.js`）是在启动时由 Vite 自动加载并执行的。无论你是运行 `npm run dev`（启动开发服务器），还是运行 `npm run build`（构建生产环境），都需要加载和执行这个配置文件。

```typescript
import { defineConfig, loadEnv } from 'vite'

// https://vitejs.dev/config/
export default defineConfig((conditionalConfig) => {
  console.log(conditionalConfig);
  console.log(process.cwd(), __dirname); 
  console.log(process.cwd() === __dirname); 
  const { mode, command, isSsrBuild, isPreview } = conditionalConfig; // conditionalConfig对象包含4个字段
  const env = loadEnv(mode, __dirname); // 根据 mode 来判断当前是何种环境
  console.log(env);
  return {
    root: process.cwd(), // 项目根目录（index.html 文件所在的位置）
	base: env.VITE_MODE === 'production' ? './' : '/', // 开发或生产环境服务的公共基础路径。
  };
})

```

`defineConfig` 中传入了一个回调函数，`conditionConfig` 里面包含了以下几个字段：

- `mode`: 当前的环境模式（如开发模式 `development` 或生产模式 `production`）。
- `command`: 当前执行的命令（如 `serve` 表示启动开发服务器，`build` 表示构建生产环境）。
- `isSsrBuild`: 是否是服务器端渲染（SSR）构建。
- `isPreview`: 是否是预览构建。

`process.cwd()`

`process.cwd()` 是一个 Node.js 内置的全局函数，它返回 **当前工作目录**（Current Working Directory）的路径。

```typescript
import { fileURLToPath, URL } from 'node:url'

import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

// https://vitejs.dev/config/
export default defineConfig((conditionConfig) => {
  console.log(conditionConfig);
  console.log(process.cwd(), __dirname);  // D:\vue_test\vue-tsx-test2 D:\vue_test\vue-tsx-test2
  console.log(process.cwd() === __dirname);  // true
  const { mode, command, isSsrBuild, isPreview } = conditionConfig; // conditionalConfig对象包含4个字段
  const env = loadEnv(mode, __dirname); // 根据 mode 来判断当前是何种环境
  const env1 = loadEnv('development', 'D:/vue_test/vue-tsx-test2');
  console.log(env);  // { VITE_MY_KEY: 'hello world' }
  return {
    plugins: [
      vue(),
      vueJsx(),
    ],
    resolve: {
      alias: {
        '@': fileURLToPath(new URL('./src', import.meta.url))
      }
    },
    server: {
      port: 3001
    }
  }
})

```

conditionConfig打印

```
{
  mode: 'development',
  command: 'serve',
  isSsrBuild: false,
  isPreview: false
}
```



### 配置环境

```typescript
import { defineConfig } from "vite";

export default defineConfig(({ command }) => {  // 将参数里的command解构出来
  if (command === "build") {
    // 返回代表生产环境的配置对象
    return {
      // 生产环境配置项
    };
  } else {
    // 返回代表开发环境的配置对象
    return {
      // 开发环境配置项
    };
  }
});

```

在vite.dev.config.ts





### 别名配置resolve.alias

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



#### 原理







## vite工具函数

### loadEnv

loadEnv()函数返回一个对象，这个对象就是根据开发模式还是生产环境加载的.env.development文件里的环境变量，有系统自带的也有自己手写的

加载 `envDir` 中的 `.env` 文件。默认情况下只有前缀为 `VITE_` 会被加载，除非更改了 `prefixes` 配置。

Vite 会根据传递给 `loadEnv` 函数的 `mode` 来加载不同的 `.env` 文件。假设你的项目中有以下环境文件：

- `.env`：默认的环境变量文件，适用于所有模式。
- `.env.development`：专门用于开发模式的环境变量文件。
- `.env.production`：专门用于生产模式的环境变量文件。

如果你运行 `vite` 或 `npm run dev`（开发模式），Vite 会加载 `.env` 和 `.env.development` 中的变量。如果你运行 `vite build`（生产模式），则会加载 `.env` 和 `.env.production` 中的变量。

`loadEnv(mode, envDir, prefixes)`

- **`mode`**：决定加载哪个模式下的环境变量文件（如 `.env.development`、`.env.production`）。

- **`envDir`**：表示 `.env` 文件所在的目录，帮助 Vite 确定环境变量文件的查找路径。

- `prefixes`：`prefixes` 参数用于过滤加载的环境变量，只加载`指定前缀`的变量。它是一个字符串数组，通常用于限制客户端可以访问的环境变量。

  如果 **`prefixes`** 参数传递一个空字符串（`''`），工具会尝试加载**所有环境变量**，不再限制前缀

```typescript
import { fileURLToPath, URL } from 'node:url'

import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

// https://vitejs.dev/config/
export default defineConfig((conditionConfig) => {
  console.log(conditionConfig);
  const { mode, command, isSsrBuild, isPreview } = conditionConfig; // conditionalConfig对象包含4个字段
  const env = loadEnv(mode, __dirname); // 根据 mode 来判断当前是何种环境
  // 等同于 
  console.log(env);
  return {
    plugins: [
      vue(),
      vueJsx(),
    ],
    resolve: {
      alias: {
        '@': fileURLToPath(new URL('./src', import.meta.url))
      }
    },
    server: {
      port: 3001
    }
  }
})
```

假设项目中的 `.env` 文件内容如下：

```env
VITE_API_URL=https://api.example.com
VITE_API_KEY=12345
SECRET_KEY=abcdef
DB_PASSWORD=supersecret
```

1. 默认加载带`VITE_`的变量

   ```javascript
   const env = loadEnv('development', './', ['VITE_']);
   console.log(env);
   ```

   ```
   {
     VITE_API_URL: 'https://api.example.com',
     VITE_API_KEY: '12345'
   }
   ```

2. 使用空字符串 `''`

   ```javascript
   const env = loadEnv('development', './', ['']);
   console.log(env);
   ```

   ```
   {
     VITE_API_URL: 'https://api.example.com',
     VITE_API_KEY: '12345',
     SECRET_KEY: 'abcdef',
     DB_PASSWORD: 'supersecret'
   }
   ```

   

当我们调用loadEnv时:

1. 直接找到`.env`文件不解释，并解析其中的环境变量，并放进一个对象里

2. 会将传进来的`mode`这个变量进行拼接，`.env.development`，并根据我们提供的目录去取相应的配置文件并进行解析，并放进一个对象里

3. 类似

   ```javascript
   const baseEnvConfig = 读取.env的配置
   const modeEnvConfig = 读取env相关配置
   const lastEnvConfig = {...baseEnvConfig, ...modeEnvConfig}
   ```

4. vite做了一个拦截，为了防止我们直接将隐私性变量直接注入`import.meta.env`中，只有`VITE_`开头变量

   可以配置`envPrefix`

5. `loadEnv` 只返回解析后的环境变量，**不会自动注入到 `process.env`**。这需要开发者手动处理。







## node

node端去读取文件或者操作文件的时候，如果发现是相对路径，则会使用process.cwd()拼接

```javascript
const fs = require("fs")
const path = require("path")  // 本质上是一个字符串处理模块，里面有非常多的路径字符串处理方法

const result = fs.readFileSync(path.resolve(__dirname,"./variable.css"))
```

为什么能直接使用__dirname

```javascript
(function(exports, require, __filename, __dirname){
  console.log(__dirname)  
}())
```







### node全局变量 / 函数

1. `__dirname`

   `__dirname` 是一个 **全局变量**，它表示当前文件所在的目录的绝对路径。

   - **定义**: `__dirname` 是 Node.js 中内置的一个变量，表示执行当前文件的目录的绝对路径。它与 `process.cwd()` 不同，`__dirname` 与当前脚本的位置有关，而 `process.cwd()` 与当前工作目录有关。
   - **常见用途**:
     - 获取当前文件所在的目录路径。
     - 在构建文件路径时很有用，特别是在文件系统操作中（如读取、写入文件）。

2. `__filename`

   当前文件的绝对路径

3. `process.cwd()`

   `process.cwd()` 是一个 Node.js 内置的全局函数，它返回 **当前工作目录**（Current Working Directory）的路径。返回当前node进程的工作目录

   - **定义**: `process.cwd()` 返回的是 **Node.js 进程启动时的工作目录**。也就是说，当你在终端执行某个命令时，操作系统会将当前所在的目录作为“工作目录”，并启动 Node.js 进程。
   - **常见用途**:
     - 用来获取当前执行脚本的工作目录（比如读取文件时，常常会依赖于工作目录）。
     - 通过 `process.cwd()` 你可以确保你知道进程从哪个目录启动，尤其在涉及文件路径时很重要。

4. `__filename`

   - **定义**: `__filename` 返回的是当前脚本文件的绝对路径，包括文件名本身。
   - **用途**: 在文件系统操作时，有时需要完整的文件路径，可以使用 `__filename`。



#### process

**`process` 不是浏览器环境下的全局变量**，它是 **Node.js 环境下的全局变量**。Vue 是一个基于浏览器的框架，而浏览器环境并没有 `process` 这个对象，因此会报错 `ReferenceError: process is not defined`。

Vite 提供了一个 `import.meta.env` 对象，它允许你在浏览器环境中访问环境变量。在 Vite 项目中，推荐使用 `import.meta.env.MODE` 来代替 `process.env.NODE_ENV`。

报错示例

```tsx
import { defineComponent, Fragment } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        console.log(process.cwd());  // 报错
        return () => (
            <div>

            </div>
        )
    }
})
```



##### process属性







## 浏览器环境变量

### import.meta.env

正式环境不存在

- import.meta.env.MODE	应用运行的模式
- import.meta.env.BASE_URL    部署应用时的基本 URL。他由`base` 配置项决定。 
- import.meta.env.PROD    应用是否运行在生产环境。
- import.meta.env.DEV    应用是否运行在开发环境。
- import.meta.env.SSR    应用是否运行在 server 上。



```tsx
import { defineComponent, Fragment } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        console.log(import.meta.env);   
        console.log(import.meta.env.MODE);  // development
        return () => (
            <div>

            </div>
        )
    }
})
```

import.meta.env打印

```json
{
    "BASE_URL": "/",
    "DEV": true,
    "MODE": "development",
    "PROD": false,
    "SSR": false,
    "VITE_MY_KEY": "hello world"
}
```



### 自定义环境变量

在项目的根目录下，新建一个` .env` 文件。 加载的环境变量也会通过 import.meta.env 以字符串形式暴露给客户端源码。 为了防止意外地将一些环境变量泄漏到客户端，<span style="color:red">Vite规定：只有以 VITE_ 为前缀的变量才会暴露给经过 vite 处理的代码。</span>

不以 `VITE_` 开头的变量不会被注入到代码中，但可以在 `vite.config.js` 中通过 `process.env` 使用

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
- `.env.production.local`，`.env.development.local`：特定模式的本地配置文件

```
# .env.development
VITE_TITLE=开发环境的标题
# .env.production
VITE_TITLE=生产环境的标题
```

然后我们可以使用 import.meta.env.VITE_TITLE，在不同的环境下渲染不同的值





### dotenv

第三方库，dotenv会自动读取`.env`文件并解析这个文件中的环境变量 并将其注入到`process`对象下（但是vite考虑到和其他配置的一些冲突问题，他不会直接注入到process对象下）

涉及到vite.config.js中的一些配置:

- root
- envDir：用来配置当前环境变量的文件地址 

我们可以调用vite的`loadEnv`方法手动确认env文件



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
    "dev": "vite --mode test"
  }
}
```



```json
  "scripts": {
    "dev": "vite"  // 实际上是 "dev": "vite --mode development"
  }
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





## 加载静态资源

vite对svg开箱即用

svg（Scalable Vector Graphics）可伸缩矢量图形：不会失真