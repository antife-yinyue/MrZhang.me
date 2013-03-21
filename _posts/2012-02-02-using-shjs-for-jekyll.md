---
layout: post
date: 2012-02-02 10:32:00
title: Using SHJS for Jekyll
tags:
- Octopress
- Plugin
- Ruby
---

我不想用Octopress自带的代码高亮插件，所以自己写了个插件，借助 [SHJS](http://shjs.sourceforge.net) 实现代码高亮。由于Haml的缩进问题，不得以把每行代码都用code标签包了下～

使用方法：`sh [:lang]`，`[:lang]`默认是`:js`。

插件代码如下：

```ruby
# To highlight source code in an HTML document using SHJS for Jekyll
# (c) {{ site.author }} | http://MrZhang.me | MIT Licensed.

require "cgi"

module Jekyll

  class SHJS < Liquid::Block

    def initialize(tag_name, markup, tokens)
      @lang = "js"
      if markup =~ /\s*:(\w+)/i
        @lang = $1
      end
      @lang = format_lang @lang
      super
    end

    def render(context)
      source = "<pre class='sh_#{ @lang }'>"
      code = CGI.escapeHTML super.lstrip.rstrip
      code.lines.each do |line|
        source += "<code>#{ line }</code>"
      end
      source += "</pre>"
    end

    def format_lang(lang)
      return "javascript" if lang == "js"
      return "ruby" if lang == "ru"
      lang
    end

  end

end

Liquid::Template.register_tag("sh", Jekyll::SHJS)
```
