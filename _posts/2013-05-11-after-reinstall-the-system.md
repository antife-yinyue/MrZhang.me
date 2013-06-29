---
layout: post
date: 2013-06-29 13:24:37 +0800
title: 重装系统之后
---

## 更改电脑名称

> 系统偏好设置 --&gt; 共享

## 安装 [Xcode](http://itunes.apple.com/us/app/xcode/id497799835) &amp; [OSX GCC Installer](https://github.com/kennethreitz/osx-gcc-installer)

## 安装 [Homebrew](http://mxcl.github.io/homebrew/)

```
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

### 安装 Git, MongoDB &amp; MySQL

```
brew install git mongodb mysql
```

设置开机自启动「可选」：

```
mkdir -p ~/Library/LaunchAgents
ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
```

## 安装 [RVM](https://rvm.io/) &amp; [Ruby](http://www.ruby-lang.org/)

```
curl -L https://get.rvm.io | bash -s stable --autolibs=enabled --ruby=1.9.3
```

## 安装 [NVM](https://github.com/creationix/nvm) &amp; [NodeJS](http://nodejs.org/)

```
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
nvm install v0.10
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

## 编辑 ~/.bash_profile

```bash
# Add RVM to PATH for scripting
PATH=$PATH:$HOME/.rvm/bin

# Load RVM into a shell session *as a function*
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"

# Load NVM
[[ -s "$HOME/.nvm/nvm.sh" ]] && . "$HOME/.nvm/nvm.sh"

# export $NODE_PATH
export NODE_PATH=$NVM_DIR/$(nvm_ls current)/lib/node_modules
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

## 安装 [Pow](http://pow.cx/)

```
gem install powder
powder install
```

## SSH-KeyGen

```
ssh-keygen -t rsa
```

## [Sublime Text 3](http://www.sublimetext.com/3)

__Bin__

```
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/sm
```

[__Package Control__](http://wbond.net/sublime_packages/package_control/installation#ST3)

```
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages
git clone https://github.com/wbond/sublime_package_control.git 'Package Control'
cd 'Package Control'
git checkout python3
```

<!--
按快捷键<code>ctrl + `</code>打开控制台，运行下面的代码后重新打开 Sublime Text 2。

```
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print('Please restart Sublime Text to finish installation')
```
-->

__Settings__

```js
{
  "ensure_newline_at_eof_on_save": true,
  "font_size": 20.0,
  "highlight_line": true,
  "line_padding_top": 5,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "trim_trailing_white_space_on_save": true
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

__Snippets__ - [Download](https://gist.github.com/jsw0528/5889931/download)

{% gist 5889931 %}
