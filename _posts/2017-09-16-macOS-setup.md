---
layout: post
title: "macOS Setup"
description: ""
category:
tags: []
---
{% include JB/setup %}

## setup
SageMath: use `.dmg` instead of `app.dmg`

```
brew install nginx --with-gunzip
brew install bash
brew install tree
brew install cmake
brew install ag
brew install ack
brew install cscope
brew install ctags
brew install nmap
brew install moreutils
brew install python3
brew install postgres
brew install ruby
brew install node

brew cask install java
brew cask install mactex
```

```
"Docker for Mac" from www.docker.com.
OmniGraffle
Acrobat Reader
Chrome Browser
SketchBook
MS Office
Atom
iTerm2
Dropbox
MacVim
Eclipse
Beyond Compare
QQ
Weixin
Shadowsocks
Proxifier
Baidu Pan
```

## Java
After using `brew cask install java` to install Java, Java updater will update Java. But Java updater will only update Java in
`/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin` whose version can be checked with `System Preferences->Java Control Panel`. It will
not touch Java in `/Library/Java/JavaVirtualMachines`.