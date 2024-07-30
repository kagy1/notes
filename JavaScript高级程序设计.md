# JavaScript高级程序设计 笔记

## 第一章：JavaScript简介

一个完整的javaScript由三部分组成：`ECMAScript`，`Bom`，`Dom`

ECMAScript(ES)

dom:  文档对象模型（Document Object Model）

bom:  浏览器对象模型（Browser Object Model）



文档对象模型（DOM，Document Object Model）是针对 XML 但经过扩展用于 HTML 的应用程序编程接口（API，Application Programming Interface）



## 第 2 章：在 HTML 中使用 JavaScript

向HTML 页面中插入 JavaScript 的主要方法，就是使用`<script>`元素

HTML 4.01 为`<script>`定义了下列 6 个属性。

- async：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或 等待加载其他脚本。只对外部脚本文件有效。 

- charset：可选。表示通过 src 属性指定的代码的字符集。由于大多数浏览器会忽略它的值， 因此这个属性很少有人用。 

- defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有 效。IE7 及更早版本对嵌入脚本也支持这个属性。 

- language：已废弃。原来用于表示编写代码使用的脚本语言（如 JavaScript、JavaScript1.2 或 VBScript）。大多数浏览器会忽略这个属性，因此也没有必要再用了。 

- src：可选。表示包含要执行代码的外部文件。 

- type：可选。可以看成是 language 的替代属性；表示编写代码使用的脚本语言的内容类型（也 称为 MIME 类型）。虽然 text/javascript 和 text/ecmascript 都已经不被推荐使用，但人 们一直以来使用的都还是 text/javascript。实际上，服务器在传送 JavaScript 文件时使用的 MIME 类型通常是 application/x–javascript，但在 type 中设置这个值却可能导致脚本被 忽略。另外，在非IE浏览器中还可以使用以下值：application/javascript和application/ecmascript。考虑到约定俗成和最大限度的浏览器兼容性，目前 type 属性的值依旧还是 text/javascript。不过，这个属性并不是必需的，如果没有指定这个属性，则其默认值仍为 text/javascript。 

  ​			

