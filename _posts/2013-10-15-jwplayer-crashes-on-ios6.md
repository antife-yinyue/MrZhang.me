---
layout: post
date: 2013-10-15 10:29:50 +0800
title: JWPlayer crashes on iOS6
---

昨天测试同学发现个 BUG，JWPlayer 在 iOS6 下会导致 Safari 崩掉，其他平台都没问题，包括 iOS7。

得益于 SeaJS 的 debug 插件，在线本地调试还是很方便的。于是一顿狂找，无果，还以为是购买的授权超限了。今天同事发来一个链接，原来也有同病相怜者。看到解决方法时，顿时呵呵了。一行 CSS 搞定：

```scss
.jwplayer { -webkit-overflow-scrolling: auto !important; }
```

![](https://raw.github.com/jsw0528/rails_emoji/master/vendor/assets/images/emojis/trollface.png ':trollface:')
