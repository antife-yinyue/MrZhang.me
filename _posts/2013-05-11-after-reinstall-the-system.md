---
layout: post
date: 2013-07-25 16:17:37 +0800
title: 重装系统之后
---

## 系统设置

* 更改电脑名称

  > 系统偏好设置 --&gt; 共享

* 允许安装任何来源的 APP

  > 系统偏好设置 --&gt; 安全性与隐私 --&gt; 通用

* 设置快捷键

  > 系统偏好设置 --&gt; 键盘 --&gt; 快捷键


## 安装 [Xcode][]

## 安装 [Homebrew][]

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

> 会提示先安装 Command Line Tools，按提示操作即可。

### 安装 Autojump, Git, MongoDB &amp; MySQL

```
brew install autojump git mongodb mysql
```

设置开机自启动「可选」：

```
mkdir -p ~/Library/LaunchAgents
ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
```

{{ site.excerpt_separator }}

## 安装 [oh-my-zsh][]

```
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

## 安装 [RVM][] &amp; [Ruby][]

```
curl -sSL https://get.rvm.io | bash --ruby=1.9.3
```

> OS X 10.9 自带 Ruby 2.0

## 安装 [NVM][] &amp; [NodeJS][]

```
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
nvm install v0.10
nvm alias default 0.10
```

## 编辑 /etc/paths

```
/usr/local/bin
/usr/local/sbin
/usr/bin
/usr/sbin
/bin
/sbin
```

## 编辑 ~/.zshrc

> __plugins=([autojump][] [git][] [osx][])__

```sh
# Customize to your needs...
PATH=$PATH:$HOME/.rvm/bin

[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"
[[ -s "$HOME/.nvm/nvm.sh" ]] && . "$HOME/.nvm/nvm.sh"

export NODE_PATH=$NVM_DIR/$(nvm_ls current)/lib/node_modules

alias ls="ls -lap"
alias pr="powder restart"
alias po="powder open"
alias pl="powder applog"
```

## 编辑 ~/.gemrc

```
:backtrace: false
:benchmark: false
:bulk_threshold: 1000
:sources:
- http://ruby.taobao.org/
:update_sources: true
:verbose: true
gem: --no-rdoc --no-ri
```

## 安装 [Pow][]

```
curl get.pow.cx | sh
```

## SSH-KeyGen

```
ssh-keygen -t rsa
```

<a id="sm"></a>

## [Sublime Text 3][]

[__Package Control__][]

__Bin__

```
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/sm
```

__Settings__

```js
{
  "ensure_newline_at_eof_on_save": true,
  "font_size": 20.0,
  "highlight_line": true,
  "line_padding_top": 5,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "trim_trailing_white_space_on_save": true,
  // Remove `-`
  "word_separators": "./\\()\"':,.;<>~!@#$%^&*|+=[]{}`~?"
}
```

__Key Bindings__

```js
[
  // Add folder to project
  { "keys": ["super+shift+o"], "command": "prompt_add_folder" },
  // Match with `ctrl+shift+k`
  { "keys": ["ctrl+shift+d"], "command": "duplicate_line" },
  { "keys": ["alt+up", "alt+1"], "command": "fold_by_level", "args": {"level": 1} },
  { "keys": ["alt+up", "alt+2"], "command": "fold_by_level", "args": {"level": 2} },
  { "keys": ["alt+up", "alt+3"], "command": "fold_by_level", "args": {"level": 3} },
  { "keys": ["alt+down"], "command": "unfold_all" }
]
```

__Snippets__ - [Download][]

{% gist 5889931 %}


[Xcode]: https://developer.apple.com/xcode/
[Homebrew]: http://brew.sh/
[oh-my-zsh]: https://github.com/robbyrussell/oh-my-zsh
[RVM]: https://rvm.io/
[Ruby]: http://www.ruby-lang.org/
[NVM]: https://github.com/creationix/nvm
[NodeJS]: http://nodejs.org/
[autojump]: https://github.com/joelthelion/autojump#readme
[git]: http://jasonm23.github.io/oh-my-git-aliases.html
[osx]: https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#osx
[Pow]: http://pow.cx/
[Sublime Text 3]: http://www.sublimetext.com/3
[__Package Control__]: https://sublime.wbond.net/installation
[Download]: https://gist.github.com/jsw0528/5889931/download
