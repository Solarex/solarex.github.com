---
layout: post
title: "Setup Mac Development Environment"
date: 2015-05-05 12:04
comments: true
categories: 
- dev
---
<p><center><img src="/images/apple_mac_logo.jpg" width=225 height=225/></center></p>

<h2 id="system-preferences">System preferences</h2>

In Apple Icon > System Preferences:

Trackpad > Tap to click
Keyboard > Key Repeat > Fast (all the way to the right)
Keyboard > Delay Until Repeat > Short (all the way to the right)
Dock > Automatically hide and show the Dock

<!-- more -->

<h2 id="homebrew">Homebrew</h2>

+ install command line tools ``xcode-select --install``,``xcode-select -p
/Library/Developer/CommandLineTools`` to check if command line tools is installed

+ install homebrew ``ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`` from [brew.sh](http://brew.sh/)

+ ``brew doctor``,``brew update``,``brew list``,``brew search wget``,``brew install wget``,``brew outdated``,``brew upgrade``,``brew upgrade wget``,``brew uninstall wget --force``,``brew info wget``,``brew deps wget``,``brew edit wget``,``brew list --versions``,``brew cleanup``

+ ``brew install caskroom/cask/brew-cask`` from [caskroom.io](http://caskroom.io/)

```bash
$ brew list
ant	  automake   coreutils	gcc49  gnu-sed	     libksba   libyaml	pkg-config  tig
apktool   brew-cask  dex2jar	git    isl011	     libmpc08  mpfr2	readline    tree
autoconf  cloog018   findutils	gmp4   libgpg-error  libtool   openssl	rename	    wget
houruhou at MacPro in ~
$ brew cask list
android-file-transfer	  dash			    go2shell		      neteasemusic		sublime-text
android-studio		  diffmerge		    google-chrome	      pycharm			vlc
appcleaner		  flux			    iterm2		      qq
bilibili		  foxmail		    jd-gui		      skim
ccleaner		  gitbook-editor	    macdown		      sogouinput
```

+ Sublime Text

```json
{
    "font_face": "Consolas",
    "font_size": 13,
    "rulers":
    [
        79
    ],
    "highlight_line": true,
    "bold_folder_labels": true,
    "highlight_modified_tabs": true,
    "tab_size": 4,
    "translate_tabs_to_spaces": true,
    "word_wrap": false,
    "indent_to_bracket": true
}
```

+ Go2Shell ``open -a go2shell --args config`` no longer needed

<h2 id="python">Python</h2>
+ ``brew install python --with-brew-openssl``,``brew install python3 --with-brewed-openssl``
+ ``sudo easy_install pip``
+ pip.conf

```bash
$ cat ~/.pip/pip.conf
; http://www.pypi-mirrors.org/
[global]
use-mirrors=true
; mirrors=http://pypi.douban.com
index-url=http://pypi.douban.com/simple
trusted-host=pypi.douban.com
; ln -s pip.conf ~/.pip/pip.conf
```

+ ``sudo pip install virtualenv virtualenvwrapper``
+ virtualenv setup

```bash
#virtualenv
# virtualenvwrapper
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python
export WORKON_HOME=/Users/houruhou/Workspace/Solarex/PythonWorkspace
[ -f /usr/local/bin/virtualenvwrapper.sh ] && source /usr/local/bin/virtualenvwrapper.sh
[ -f /etc/bash_completion.d/virtualenvwrapper ] && source /etc/bash_completion.d/virtualenvwrapper
export PIP_VIRTUALENV_BASE=$WORKON_HOME
export PIP_RESPECT_VIRTUALENV=true
```

+ ``mkvirtualenv --no-site-packages test``

<h2 id="java">Java</h2>

+ open eclipse prompt to install java

```bash
/Library/Java/JavaVirtualMachines/jdk1.7.0_45.jdk/Contents/Info.plist

diff

<key>JVMCapabilities</key>
    <array>
<string>CommandLine</string>
</array>

<key>JVMCapabilities</key>
<array>
    <string>JNI</string>
    <string>BundledApp</string>
    <string>WebStart</string>
    <string>Applets</string>
    <string>CommandLine</string>
</array>
```

+ ``eclipse.ini``

```bash
--launcher.XXMaxPermSize
2048m
...
-vmargs
...
-Xms512m
-Xmx850m
-XX:PermSize=512m
-XX:MaxPermSize=1024m
```

+ [mac-dev-setup](https://github.com/nicolashery/mac-dev-setup)
+ [mac-dev](https://github.com/pubyun/macdev)
+ [setup-mac-dev-zh_cn](https://aaaaaashu.gitbooks.io/mac-dev-setup/content/SystemPreferences/index.html),[setup-mac-dev](http://sourabhbajaj.com/mac-setup/)
+ [mac-configurations](https://github.com/solarex/macconfigurations)





