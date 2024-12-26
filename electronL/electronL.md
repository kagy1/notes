# electron

## 安装electron

1. 安装vue

2. 安装electron

   ```
   npm install electron
   ```

   如果安装过程中卡住

   ```
   npm config edit  // 打开npm 配置文件
   ```

   在打开的配置文件空白处将下面几个配置添加上去,注意如果有原有的这几项配置，就修改

   ```
   registry=https://registry.npmmirror.com
   electron_mirror=https://cdn.npmmirror.com/binaries/electron/
   electron_builder_binaries_mirror=https://npmmirror.com/mirrors/electron-builder-binaries/
   ```

   关闭窗口，删除node_modules,并且清除npm缓存

   ```
   npm cache clean --force
   ```

3. 在根目录创建 `electron` 文件夹，新建 `index.html` 文件，添加如下代码：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   
   <head>
       <meta charset="UTF-8" />
       <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'" />
       <meta http-equiv="X-Content-Security-Policy" content="default-src 'self'; script-src 'self'" />
       <title>Hello from Electron renderer!</title>
   </head>
   
   <body>
   
   </body>
   
   </html>
   ```

4. 在 `electron` 目录下新建文件 `main.js`，并添加如下代码：

   ```javascript
   import { createRequire } from 'module';   // 从node.js 14版及以上版本中，require作为COMMONJS的一个命令已不再直接支持使用，所以我们需要导入createRequire命令才可以
   const require = createRequire(import.meta.url);
   
   const { app, BrowserWindow } = require('electron')
   const path = require("path")
   
   const createWindow = () => {
       const mainWindow = new BrowserWindow({
           width: 800,
           height: 600,
       })
   
       mainWindow.loadURL("http://localhost:3000/");
   }
   
   // 在应用准备就绪时调用函数
   app.whenReady().then(() => {
       createWindow()
   })
   ```

5. 修改 `package.json` 文件，指定 `electron/main.js` 为 `Electron` 的入口文件，并添加 `ele` 命令以开发模式运行 `Electron`：

   ```json
   {
     "main": "electron/main.js",
     "scripts": {
       "ele": "vite | electron ."
     }
   }
   ```

   



