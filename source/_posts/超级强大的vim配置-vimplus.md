---
title: 超级强大的vim配置(vimplus)
date: 2016-10-20 22:37:14
categories: vim
tags: 
    - vim
    - github
    - linux
---

![此处输入图片的描述][1]

前言
-----

vim和emacs是linux环境下的文本编辑利器，关于vim和emacs谁更优秀的话题从来没有断过，我在这里就不再评判了，vim是linux下的默认编辑器，学好了vim将会一生受用，我之前学vim是在网上找的一些资料，读博客之类的，使用了几年vim始终感觉没有什么大的进步，后来在vim官网看到vim书籍推荐，其中一本就是《vim实用技巧》，后来果断在京东上买了一本，除了宏相关的没怎么看以外，其他的都看了，加上自己的实际操作，感觉vim技术又上了一个层次，《vim实用技巧》是教会vimer怎么使用vim，使用vim编辑一些大型项目时，给vim装上一些插件，将会如虎添翼，后来我在网上找一些插件来安装，或者在github上搜索别人的vimrc，看别人装了什么插件，自己选择性的安装了一些，使用一段时间后感觉使用vim编辑代码就是一件非常愉快的事情，再加上我最近买的忍者二代机械键盘那简直写代码很带感啊，我最开始自己家的电脑上给vim装了很多插件，后来在公司又要重新很麻烦的搭建vim开发环境，感觉有点麻烦，后来又想有没有什么一键安装、部署之类的小程序，就可以傻瓜式的把开发环境给搭建起来不是很爽吗，vimplus就运运而生了，废话不多说，直接上安装步骤。

<!--more-->

安装
-----

```bash
git clone https://github.com/chxuan/vimplus.git
cd ./vimplus
sudo ./install.sh
```

现在vimplus支持ubuntu14.04之后的所有ubuntu 64位系列以及centos7 64位，运行`install.sh`脚本，你就可以一边喝咖啡，一遍看着屏幕刷刷刷的打印就安装部署好了开发环境了，整个过程大约持续40分钟，其中下载编译ycm耗费了大半时间。


  [1]: https://raw.githubusercontent.com/chxuan/vimplus/master/screenshots/main.png
