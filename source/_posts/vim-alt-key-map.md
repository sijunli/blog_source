---
title: 如何在vim中添加Alt键开头的快捷键映射
toc: true
date: 2016-06-15 23:34:25
tags:
- wiki
- vim
---

## 前面的话

 最近想好好把python学一学，计划是先学习一下flask，结合vue做一些简单的网站之类，之后再学学爬虫、图像处理、机器学习之类的东西。看了一下大家写Python好像没有什么比较推崇的IDE，又不像java那么依赖IDE做补全和重构，加上自己之前用VIM写python感觉特别流畅，所以就想用VIM自己搭一个python开发环境。

参考了一个youtube上标题貌似是“Use vim as python IDE”的视频，学着上面的指导安装了python-mode、ctrlP、jedi、powerline和rope这些vim插件，但是在装完rope使用的时候，遇到了一个问题，就是rope的代码补全帮助 (RopeCodeAssist命令) 不能用，作为rope里我认为最牛逼的一个功能没有之一，不能用的话也太让人揪心了。

整了好几个小时以后发现不是rope的问题，是vim里面快捷键的问题。我在vim的普通模式直接输`:RopeCodeAssist`命令是可以用的，但是通过快捷键`<M-/>`去没有，而是出来了搜索功能，就想是vim中普通模式下按`/`的效果。原因是alt开头的快捷键在xshell中映射得不对。

## vim中的快捷键映射

vim的设置中，可以在.vimrc文件中使用`map`命令设置键位映射，如在.vimrc中增加一行`map a b`，那么在普通模式下输入`a`就会起到`b`的效果（在vim按`b`是光标向后移动一个词）。更tricky一点，用`inoremap a b`，那么在编辑模式下输入的a字符全都会变成b。

如果要表示一个ctrl开头的快捷键，可以用<C表示，如在map命令中，`C-r`表示快捷键ctrl+r，用法是先按住ctrl不放，再按r键。相应的<M和<A都是表示Alt键，<S表示shift键。

## 问题解决

按理说RopeCodeAssist这个命令映射到了`<M-/>`上，但是实际上按下alt+/以后vim并没有收到`<M-/>`按键值，而是收到`<ESC>/`。这样就很容易理解前面的现象了：在编辑模式按下`<ESC>`进入普通模式，在普通模式按下`/`进入搜索模式。

为什么会输入值不是期待的值呢？如果用过emacs会有这样的经验，<M-b>和<ESC>b的效果是一样的，特别是在Mac这种没有meta和alt键的机器上，只能用<ESC>完成原本meta键的工作。如果符合这个设定的话，那上面的行为是正确的。但是或许在mac和linux上，vim中接收到的<M-/>字符会是一个特殊字符吧，与实际输入的`<ESC>/`不相符合。

解决的方法是在vimrc中把<M-/>的值设成<ESC>/，这样以后接收到`<ESC>/`就会触发<M-/>，相当于按下了<M-/>，不管是通过alt+/输入的，还是单独按<ESC>和/输入的。设置的方法如下。

```
" ^[不是单独的两个字符，而是一个字符，是真实的ESC键取值，在vim中可以通过先按<C-v>，然后按<ESC>输入。
set <M-/>=^[/ " 用于触发RopeCodeAssist命令
set <M-?>=^[? " 用于触发RopeLuckyAssist命令
```

