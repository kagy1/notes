# node

## 包管理工具

### npm

#### 命令

1. npm init

   `npm init` 命令用于初始化一个新的npm项目。它会引导你创建一个`package.json`文件，其中包含了项目的基本信息和依赖项。这个文件是项目的心脏，记录了项目的元数据和依赖关系。

2. npm install

   `npm install` 命令用于安装新的包。它可以安装来自npm仓库的任何包，并将它们添加到项目的依赖列表中。

   - 全局安装  -g

     ```
     npm install -g <package_name>
     ```

   - 本地安装

     ```
     npm install <package_name>
     ```

   - 作为开发依赖安装

     如果你安装的包仅用于开发环境，例如测试框架，你可以使用`--save-dev`或`-D`标志。

     ```
     npm install --save-dev <package_name>
     npm install -D <package_name>
     ```

   - 不保存安装的包

     默认情况下，`npm install`会将安装的包保存到`package.json`文件中。如果你想避免自动保存，可以使用`--no-save`标志。

     ```
     npm install <package_name> --no-save
     ```

3. npm uninstall

   `npm uninstall` 命令用于从项目中移除依赖包。它会从`package.json`文件中删除指定的包，并卸载它。

   ```
   npm uninstall <package_name>
   ```

   如果你想同时移除全局包和本地包，可以使用`-g`标志。

   ```
   npm uninstall -g <package_name>
   ```

4. npm update

   `npm update` 命令用于更新项目中的依赖包。它可以更新所有依赖包，也可以指定更新某个特定的包。

   - 更新所有依赖

     ```
     npm update
     ```

   - 更新特定依赖

     ```
     npm update <package_name>
     ```

5. npm list

   `npm list` 命令用于列出当前项目中安装的包及其依赖关系。

   ```
   npm list
   npm list --global  查看全局安装的包
   ```

6. npm search

   `npm search` 命令用于搜索npm仓库中的包。

   ```
   npm search <query>
   ```

7. npm view

   `npm view` 命令用于查看指定包的详细信息，包括版本、依赖、描述等。

   ```
   npm view <package_name>
   ```

8. npm ls

   `npm ls` 命令用于列出当前项目中安装的包的版本树。

9. npm cache

   `npm cache` 命令用于管理npm的缓存。有时候，清理缓存可以解决一些安装问题。

   ```
   npm cache dir  // 查看缓存位置
   npm cache clean --force  // 清理缓存
   ```

10. npm config

    `npm config` 命令用于设置或查看npm的配置选项。

    - 打开npm 配置文件

      ```
      npm config edit
      ```

    - 设置一个新的配置选项

      ```
      npm config set <key> <value>
      npm config set registry https://registry.npm.taobao.org
      ```

    - 查看配置

      ```
      npm config list
      ```

11. npm help

    npm help 命令用于获取`npm命令`的帮助信息

    ```
    npm help <command>
    ```

    

#### 包如何到达node_modules

1. 解析依赖树：npm首先会根据package.json文件中的依赖列表，以及可能存在的锁定文件（如package-lock.json）来解析项目的依赖树。从而确定需要安装的包以及版本要求
2. 检查本地缓存：npm会检查本地缓存（位于用户主目录下 .npm 目录）是否已经存在所需的包。如果包已经存在于缓存中且版本符合要求，则npm会直接从缓存中复制到node_modules目录，跳过后续步骤。
3. 请求包并下载：如果包不在本地缓存中，npm 将向远程软件包注册表发送请求，以获取包的元数据和下载链接。npm默认使用官方的npm注册表，也可以配置使用其他私有或定制的注册表。
4. 下载和解压包：npm根据获取到的下载链接，将包的压缩文件下载到临时目录中。然后，npm会解压缩该文件，并将解压后的文件移动到node_modules目录中。
5. 安装包的依赖项：对于每个安装的包，npm会检查它们的package.json文件，查找并安装它们的依赖项。这个过程会递归进行，直到所有的依赖项都被安装到node_modules目录中。
6. 更新锁定文件：（可选）如果使用了锁定文件（如package-lock.json），npm会更新锁定文件以记录确切安装的版本，以便在将来重现相同的依赖关系。



#### 包管理规则

1. 扁平化结构：npm尽可能地将依赖包安装到node_modules顶层。这意味着，即使多个依赖项都需要同一个包的不同版本，也会安装每个版本的单独副本，而不会形成嵌套的依赖关系。
2. 依赖解析：当一个模块引用另一个模块时，npm会根据require语句中的模块标识符来解析依赖关系。它会首先在当前模块的node_modules目录下查找被引用的模块，如果找不到，会逐级向上查找，直到顶层的node_modules目录。
3. 包版本冲突解决：npm使用`依赖树`和`依赖优先级`的概念来解决这个问题。当多个依赖项需要不同版本的包时，npm会根据依赖树的结构选择合适的版本。
4. 锁定文件：如package-lock.json，记录了确切安装的包及其版本。锁定文件可以确保在重复安装相同依赖关系时，使用相同的版本，从而保持一致性。
5. 嵌套依赖：当一个依赖项依赖于特定版本的另一个依赖项时，npm会在node_modules目录内创建子目录，将被依赖项安装在子目录下。



### yarn

在npm5之前，npm存在很多缺陷。yarn用来代替npm。

### pnpm

速度快，节省磁盘空间。

如果有100个项目，使用相同的依赖包，那么硬盘上会需要保存100份该依赖包的副本，pnpm会放在一个统一的位置。



### npx

npx是一个工具，npm 从 v5.2.0 开始新增了 npx 命令，>= 该版本会自动安装 npx，是一个npm包执行器，旨在提高从npm注册表使用软件包的体验 ，npm 使得它非常容易地安装和管理托管在注册表上的依赖项，npx 使得使用 CLI 工具和其他托管在注册表，加强了用户的体验。

Node安装后自带npm模块，可以直接使用npx命令。如果不能使用，就要手动安装一下。

```
npm install -g npx
```





