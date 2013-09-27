---
layout: post
date: 2012-08-02 13:55:00 +0800
title: Hello, SeaJS!
---

{% assign issues = 'https://github.com/seajs/seajs/issues' %}

不知道这货是啥？那你就 OUT 啦～

SeaJS 是一款适用于 Web 浏览器端的模块加载器，它同时又[与 Node 兼容]({{ issues }}/275)。在 SeaJS 的世界里，一个文件就是一个模块，所有模块都遵循[CMD（Common Module Definition）]({{ issues }}/242)规范。一个模块基本是长这样的：

```js
define(function(require, exports, module) {
  // Your codes here
})
```

## 一、如何使用？

**Step1:** 在页面引入`sea.js`。为了让`sea.js`内部能快速获取到自身路径，推荐手动加上`id`属性：

```js
<script src="http://path/to/seajs/1.2.0/sea.js" id="seajsnode"></script>
```

**Step2:** 使用`seajs.use`加载模块文件：

```js
<script>
seajs.use('./hello')

// 可以带 callback
seajs.use('./hello', function(hello) {
  hello.api()
})

// 也可同时（依次）加载多个模块
seajs.use(['./hello', './world'], function(hello, world) {
  hello.api()
  world.api()
})
</script>
```

你应该已经注意到，被加载的模块文件都没带后缀，那是因为 SeaJS 默认会给没有指定后缀的自动补上`.js`后缀。但有两种情况是不会自动添加的，一是路径以井号`#`结尾，二是路径中含有问号`?`。

{{ site.excerpt_separator }}

## 二、模块标识

模块标识是一个字符串，用来标识模块。上面提到的「路径」就是一种模块标识。模块标识和模块是一一对应的，可以理解为模块的唯一识别码。SeaJS 有三种模块标识：相对标识、顶级标识、普通路径。

### 1. 相对标识

相对标识以`./`或`../`开头，只出现在模块文件内。它永远相对当前模块的 URI 来解析。

### 2. 顶级标识

顶级标识不以`.`或`/`开始，它会相对 SeaJS 的`base`路径来解析。
那这个`base`路径又是什么呢？它是通过`sea.js`的访问路径得到的，比如访问路径是`http://path/to/seajs/1.2.0/sea.js`，那`base`路径就是`http://path/to/`。

### 3. 普通路径

除了相对和顶级标识之外的标识都是普通路径。普通路径的解析规则，和 HTML 代码中的`<script src="..."></script>`一样，会相对当前页面解析。通过`seajs.use`加载的模块，除非使用了`alias`，不然始终会以「普通路径」的规则来解析。

## 三、`seajs.config({})`

不能让用户配置的工具都不是好工具。SeaJS 具体有哪些配置项，可以参见[这里]({{ issues }}/262)。

需要说明的是，`seajs.config`是可以多处调用的，同名 key 覆盖，不同名的 key 则叠加。这样就可以有全局配置和细分配置了。

最最常用的配置项，要数`alias`了，它可以简化模块标识，还可以用作版本管理。如果配置的别名已经存在，SeaJS 会提示`The alias config is conflicted: key="..." previous="..." current="..."`，表示别名冲突，那就需要人肉检查是否会影响其他模块了。

```js
seajs.config({
  alias: {
    'app': 'http://path/to/app',    // 普通路径
    'jquery': 'jquery/1.7.2/jquery' // 顶级标识
  }
})
```

SeaJS 从 v1.2.0 开始，类似上面 jQuery 的写法可以简化为`{ 'jquery': '1.7.2' }`。

## 四、具体的模块文件如何写？

至此，你基本已经知道如何使用 SeaJS 了，下面讲讲模块文件。

对于本文一开始告知的模块基本格式，我的建议是，不要做任何修改、赋值，不仅是`define`和`require`，还包括`exports`和`module`。

- `require('模块标识')` 用来获取其他模块提供的接口。
- `exports` 用来对外提供接口。
- `module` 对象存储了与当前模块相关联的一些属性和方法。

更具体的规范介绍，参见[官方文档]({{ issues }}/242)。

其他代码你原来怎么写，现在还是怎么写。就这么简单。

最后，恭喜，你已经入门了，尽情享受 SeaJS 吧～
