 # 微前端 micro-app

## 基本概念

微前端（Micro Frontends）是一种前端架构模式，通过将单个应用程序分解为多个小型、独立的部分来实现应用程序的组合。每个小型部分都由独立的团队开发、测试和部署，然后将它们组合成为一个完整的应用程序。

微前端的目标是使前端开发更加容易、可维护和可扩展，并且能够实现团队之间的协作。

在微前端架构中，每个微前端都有自己的代码库和独立的部署过程。

微前端可以使用不同的技术栈、框架和语言，因为它们只需要定义一组共享的 API 和协议。这样可以让团队独立地开发和部署微前端，同时还能够保持整个应用程序的一致性。



## 框架

iframe、Single-spa、Qiankun、Micro-app

1. iframe html标签

```js
<iframe src="xxx" sandbox></iframe>
```

使用iframe有三个难以解决的问题，

   - **路由状态丢失**，刷新一下，iframe的url状态就丢失了
   - **dom割裂严重**，弹窗只能在iframe内部展示，无法覆盖全局
   - **通信非常困难**，只能通过postmessage传递序列化的消息



2. Single-spa
   - 注册应用
   - 挂载到基座
   - 监听url变化激活对应的子应用

- 改造成本大

- 沙箱不完美

- 应用通信能力差

- 不支持es model，不支持vite

  

3. Qiankun



4. Micro-app

micro-app并没有沿袭single-spa的思路，而是借鉴了`WebComponent`的思想，通过CustomElement结合自定义的ShadowDom，将微前端封装成一个类WebComponent组件，从而实现微前端的组件化渲染。并且由于自定义ShadowDom的隔离特性，micro-app不需要像single-spa和qiankun一样要求子应用修改渲染逻辑并暴露出方法，也不需要修改webpack配置，是目前市面上接入微前端成本最低的方案。

- 使用简单
  将所有功能都封装到一个类WebComponent组件内，从而实现在基座应用中嵌入一行代码即可渲染一个微前端应用。
  同时micro-app还提供了js沙箱、样式隔离、元素隔离、预加载、数据通信、静态资源补全等一系列完善的功能。
- 零依赖
  micro-app没有任何依赖，这赋予它小巧的体积和更高的拓展行。
- 兼容所有框架
  为了保证各个业务之间独立开发、独立部署的能力，micro-app做了诸多兼容，在任何技术框架中都可以正常运行。

- WebComponent：原生组件
- CustomElement：自定义元素
- ShadowDom：影子DOM