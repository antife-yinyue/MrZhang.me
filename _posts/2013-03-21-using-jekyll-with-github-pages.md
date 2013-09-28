---
layout: post
date: 2013-05-28 08:00:00 +0800
title: Using Jekyll with GitHub Pages
ghbtns:
- jekyll-cli
- MrZhang.me
---

{% include ghbtns.html %}
{% assign jekyll_cli = '[`jekyll-cli`](https://github.com/jsw0528/jekyll-cli)' %}


我不喜欢，也不擅长码字，我只是喜欢折腾。

去年我用 [Octopress]({{ site.url }}/blog/blog-equals-github-plus-octopress.html)，慢慢发现，除了我自己为它写的插件，其他插件基本没用上。So，我为什么不直接用 [Jekyll](http://jekyllrb.com/) 呢，还省了静态化的过程，这种琐事直接交给 GitHub 得了。而且也不需要另建分支备份源码了，一举两得啊！

为了方便 You &amp; Me，我弄了个小工具{{ jekyll_cli }}{{ ghbtns[0] }}，跑在 NodeJS 上。接下来我就以{{ jekyll_cli }}为引，简单介绍下如何使用 GitHub 原生支持的 Jekyll。

## 在本地创建一个 Jekyll 项目

Jekyll 是跑在 [Ruby](http://www.ruby-lang.org) 上的，所以你得先确保你机子已经能跑 Ruby。最好 [Bundler](http://gembundler.com) 也装好了：

```bash
$ gem install bundler
```

接着{{ jekyll_cli }}上场，新建项目很简单：

```bash
$ jkl new path/to/blog
```

这样，在你指定的地方会创建一个名为`blog`的文件夹，里面包含了 Jekyll 所需的基本目录结构：

```
|-- _config.yml
|-- _layouts/
|   |-- default.html
|   `-- post.html
|-- _posts/
|   |-- 2013-03-21-hello-world.md
`-- Gemfile
`-- index.html
```

执行`jkl new`命令后会自动执行`bundle install`，如果想稍后手动安装 gems，请使用：

```bash
$ jkl new path/to/blog --no-bundle
```

{{ site.excerpt_separator }}

## 创建文章

`_posts/` 目录是存放文章源文件的地方，你可以按照约定格式`YEAR-MONTH-DAY-title.MARKUP`手动创建文件，或者使用{{ jekyll_cli }}：

```bash
$ cd path/to/blog
$ jkl post '文章标题'
```

你可以指定文件后缀名：

```bash
$ jkl post '文章标题' --ext html
```

或者作为草稿（Jekyll v1.x 新增功能）：

```bash
$ jkl post '文章标题' --drafts
```

## 本地预览

```bash
$ jkl watch
```

默认端口号是`4000`，当然你可以在`_config.yml`中设置`port: 8080`，或者直接使用命令行：

```bash
$ jkl watch --port 8080
```

默认情况下，render posts 的时候，会把`_drafts/`目录内的草稿也包含进去，但你可以手动排除：

```bash
$ jkl watch --no-drafts
```

{{ jekyll_cli }}有个贴心功能，就是使用默认浏览器自动打开本地预览地址：

```bash
$ jkl watch --open
```

本地服务跑起来之后，Jekyll 会自动创建`_site/`目录，把`blog/`目录内__不是__以下划线开头的文件（夹）全部 Copy 进来，Markdown 文件也会编译成 HTML。

在`Gemfile`文件内，有一行代码`gem 'redcarpet'`，这个 gem 是编译 Markdown 用的，[GitHub 也在用](https://github.com/blog/832-rolling-out-the-redcarpet)，而且有了它还可以使用超棒的 [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown)。

### 预编译 Sass

我喜欢 [Sass](http://sass-lang.com)，所以{{ jekyll_cli }}也就有了这个功能。命令就是：

```bash
$ jkl sass
```

设计博客外观的时候，Sass 文件频繁改动，希望能自动执行编译操作，使用：

```bash
$ jkl sass --watch
```

编译时使用的参数，可以在`_config.yml`中设置，具体有哪些参数可用，可以在终端执行`compass help compile`查看。写在`_config.yml`内的参数的格式跟终端显示的有一些区别，就是：去掉`--`前缀，然后把连字符`-`改成下划线`_`，如果没有参数值的，用布尔值`true`跟`false`代替。

## 连接 GitHub

在 GitHub 上新建一个仓库，然后在终端执行：

```bash
$ jkl git
```

终端会提示你输入具有读写权限的仓库地址，示例地址见下图：

![](https://github-images.s3.amazonaws.com/help/remotes-url.png)

## 部署

```bash
$ jkl deploy
```

该命令会将`blog/`目录内的文件（夹）push 到你刚刚创建的仓库中。本地预览时生成的`_site/`目录是不需要的，建议写进`.gitignore`。

### 写在最后

{{ jekyll_cli }}的命令，除了`jkl new`，其他都需要在`blog/`目录内执行。

更多命令可以运行`jkl -h`查看。列出某个命令所有 options，使用`jkl [command] -h`，比如`jkl post -h`。

本文只是简单介绍，想要更深入的使用 Jekyll，需要你自己看[文档](http://jekyllrb.com/docs/home/)。

你可以为你的博客[绑定自己的域名](https://help.github.com/articles/setting-up-a-custom-domain-with-pages)，还可以[自定义404页面](https://help.github.com/articles/custom-404-pages)。

上面说的这些，应该不难吧？有什么没有表达清楚的地方么？你也可以研究下[我博客的源码](https://github.com/jsw0528/MrZhang.me) {{ ghbtns[1] }}，或许可以给你一点点的帮助。

有任何疑问和心得，欢迎留言交流。
