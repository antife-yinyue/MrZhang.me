---
layout: post
date: 2012-09-27 10:33:00
title: SPM in eDoctor
tags:
- SeaJS
- SPM
---

本文不是 SPM 的文档。

关于 SPM 有的没的，可以看 [Wiki](https://github.com/seajs/spm/wiki)。

## 「一键」安装

考虑到让新装 Linux 系统也能安装 NodeJS + SPM，写了个 Shell 脚本，使用时把末尾的`NODE_VERSION`替换为真实的[NodeJS 版本号](http://nodejs.org/download)，比如现在的`0.8.10`。

```bash
$ curl https://raw.github.com/eDoctor/install-nodejs-and-spm/master/install-nodejs-and-spm.sh | bash -s NODE_VERSION
```

<!-- more -->

## JS 存放目录结构

```
 └─ modules/　　　　　　　　　　　　　　　存放通用模块
 　└─ seajs-config.js　　　　　　　　　　主要是配置 alias，也给 spm build 用
 　└─ seajs/
 　└─ jquery/
 └─ shared/　　　　　　　　　　　　　　　 各项目公共代码片段
 　└─ oo.js
 　└─ xx.js
 └─ app-1/　　　　　　　　　　　　　　　　# 项目一
 　└─ package.json　　　　　　　　　　　 配置文件 for spm build
 　└─ src/
 　　└─ shared -> ../../shared　　　　 软链
 　　└─ x.js
 　　└─ y/
 　　　└─ z.js
 └─ app-2/　　　　　　　　　　　　　　　　# 项目二
 　└─ package.json
 　└─ src/
 　　└─ a.js
 　　└─ b.js
 　　└─ c.js
 └─ app-N/　　　　　　　　　　　　　　　　# 项目N
```

## package.json

每个项目下都有这个文件，它是控制`spm build`输出的关键。比如上面项目一的配置，代码如下：

```js
{
  "name": "app-1",
  "dependencies": {
    "$": "#jquery/1.8.1/jquery"
  },
  "output": {
    "x": "*",
    "y/z": "*",

    "*": {
      "excludes": ["#jquery/1.8.1/jquery"]
    }
  }
}
```

打包后，模块的 ID 会被替换为全路径，因此 production 环境下不再需要载入`seajs-config.js`。`jquery.js`会和`sea.js`一起在服务器上自动 combo，以便全站缓存，所以打包后的 JS 是不需要合并进`jquery.js`的。因此，配置好 jQuery 的版本后，还需要排除掉。

`output`的 key 是相对于`src/`的路径，可以省略`.js`后缀。

更多关于`output`规则的介绍，[请移步这里](https://github.com/seajs/spm/wiki/SPM-配置详解之output篇)。

## spm build

```bash
$ cd path/to/app-1
$ spm build --with-debug='' --version=RELEASE_NAME --dist=RELEASE_NAME --global-config=/path/to/seajs-config.js
```

上面的命令会作为整个部署流程的一个 task。每个参数的含义如下：

- `--with-debug` - 不生成合并了但没压缩的 JS
- `--version` - 用作单入口下的「版本」管理
- `--dist` - 指定输出目录
- `--global-config` - 载入 alias 配置，避免在`package.json`重复配置

e.g.

```js
// x.js
define(function(require, exports, module) {
  var $ = require('$')

  $(function() {
    $(document.body).text(
      require('./shared/oo').name
    )
  })
})

// y/z.js
define(function(require, exports, module) {
  seajs.log('http://MrZhang.me/')
})

// shared/oo.js
define(function(require, exports, module) {
  exports.name = '{{ site.author }}'
})
```

假设部署时生成的`RELEASE_NAME`为`123456`，那么打包后的 JS 为：

```js
// x.js
define("http://edrjs.com/app-1/123456/shared/oo",[],function(e,t,n){t.name="{{ site.author }}"}),define("http://edrjs.com/app-1/123456/x",["./shared/oo","#jquery/1.8.1/jquery"],function(e,t,n){var r=e("#jquery/1.8.1/jquery");r(function(){r(document.body).text(e("./shared/oo").name)})});
// y/z.js
define("http://edrjs.com/app-1/123456/y/z",[],function(e,t,n){seajs.log("http://MrZhang.me/")});
```

SPM 已经自动计算好依赖关系，并替换掉 alias。是不是发现 ID 上多了个`edrjs.com`？那是因为在家目录下有个全局配置文件`~/.spm/config.json`，在这上面配置了`root`。

## 平时开发时的注意事项

1. 注意好 JS 存放位置
2. 增、减 JS 文件后注意调整`package.json`的`output`
3. 配合相关[Helpers](https://github.com/eDoctor/eRails/blob/master/Helpers.md#helpers)，只需关注代码本身即可
