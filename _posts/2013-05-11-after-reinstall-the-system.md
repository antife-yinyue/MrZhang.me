---
layout: post
date: 2013-07-25 16:17:37 +0800
title: 重装系统之后
---

## 系统偏好设置

* 更改电脑名称

  > 共享

* 允许安装任何来源的 APP

  > 安全性与隐私 --&gt; 通用

* 设置快捷键

  > 键盘 --&gt; 快捷键


## 安装 [Xcode][]

## 安装 [Homebrew][]

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

> 会提示先安装 Command Line Tools，按提示操作即可。
> 或者也可以预先手动执行`xcode-select --install`安装。

{{ site.excerpt_separator }}

## 安装 git, [autojump][]

```
brew install git autojump
```

## 安装 [oh-my-zsh][]

```
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

> __plugins=(autojump [git][] [osx][])__

## 安装 [rbenv][]

```
brew install rbenv ruby-build rbenv-gemset
```

在`~/.zshrc`中添加：

```
eval "$(rbenv init -)"
```

## 安装 Ruby

> OS X 10.9 自带 Ruby 2.0

```
rbenv install -l     # list all available versions
rbenv install 2.1.0  # install a Ruby version
rbenv global 2.1.0   # set the global version
rbenv versions       # list all installed Ruby versions
```

## 安装 MongoDB, MySQL

```
brew install mongodb mysql
```

设置开机自启动「可选」：

```
mkdir -p ~/Library/LaunchAgents
ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
```

<a id="nodejs"></a>

## 安装 [NVM][]

```
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

在`~/.zshrc`中添加：

```
[[ -s "$HOME/.nvm/nvm.sh" ]] && . "$HOME/.nvm/nvm.sh"
export NODE_PATH=$NVM_DIR/$(nvm_ls current)/lib/node_modules
```

## 安装 NodeJS

```
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

## 编辑 ~/.gemrc

```
:backtrace: false
:benchmark: false
:bulk_threshold: 1000
:sources:
- http://ruby.taobao.org
:update_sources: true
:verbose: true
gem: --no-document --prerelease
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
  "font_size": 20.0,
  "highlight_line": true,
  "line_padding_top": 5,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "trim_trailing_white_space_on_save": true,
  "trim_automatic_white_space": false,
  "ensure_newline_at_eof_on_save": true,
  "default_line_ending": "unix",
  "show_encoding": true,
  "open_files_in_new_window": false,
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
[autojump]: https://github.com/joelthelion/autojump
[oh-my-zsh]: https://github.com/robbyrussell/oh-my-zsh
[git]: http://jasonm23.github.io/oh-my-git-aliases.html
[osx]: https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#wiki-osx
[rbenv]: https://github.com/sstephenson/rbenv
[NVM]: https://github.com/creationix/nvm
[Pow]: http://pow.cx/
[Sublime Text 3]: http://www.sublimetext.com/3
[__Package Control__]: https://sublime.wbond.net/installation
[Download]: https://gist.github.com/jsw0528/5889931/download
