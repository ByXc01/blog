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

vim和emacs是linux环境下的文本编辑利器，关于vim和emacs谁更优秀的话题从来没有断过，我在这里就不再评判了，vim是linux下的默认编辑器，学好了vim将会一生受用，我之前学vim是在网上找的一些资料，读博客之类的，使用了几年vim始终感觉没有什么大的进步，后来在vim官网看到vim书籍推荐，其中一本就是《vim实用技巧》，后来果断在京东上买了一本，除了宏相关的没怎么看以外，其他的都看了，加上自己的实际操作，感觉vim技术又上了一个层次，《vim实用技巧》是教会vimer怎么使用vim，使用vim写代码时，给vim装上一些插件，将会如虎添翼，后来我在网上找一些插件来安装，或者在github上搜索别人的vimrc，看别人装了什么插件，自己选择性的安装了一些，使用一段时间后感觉使用vim编辑代码就是一件非常愉快的事情，再加上我最近买的忍者二代机械键盘那简直写代码很带感啊，我最开始自己家的电脑上给vim装了很多插件，后来在公司又要重新搭建vim开发环境，感觉有点麻烦，后来又想有没有什么一键安装、部署之类的小程序，就可以傻瓜式的把开发环境给搭建起来不是很爽吗，[vimplus][2]就运运而生了，如果喜欢的朋友请不要吝啬，给个star，废话不多说，直接上安装步骤(个人博客也发表了[《超级强大的vim配置(vimplus)》][3])。

<!--more-->

安装
-----

```bash
git clone https://github.com/chxuan/vimplus.git
cd ./vimplus
sudo ./install.sh
```

现在vimplus支持ubuntu14.04之后的所有ubuntu 64位系列以及centos7 64位，运行`install.sh`脚本，你就可以一边喝咖啡，一遍看着屏幕刷刷刷的打印就安装部署好了开发环境了，整个过程大约持续40分钟，其中下载编译ycm耗费了大半时间，我有下载好了的[YouCompleteMe.tar.gz][4]，省得在github上去下载，很慢的，你懂的，若想要手动安装ycm，需要修改`vimplus`目录下的`.vimrc`文件。

```bash
Plugin 'Valloric/MatchTagAlways'
#Plugin 'Valloric/YouCompleteMe'
Plugin 'docunext/closetag.vim'
```

将ycm插件那行注释掉，不然还会再去下载ycm，ycm可以最后等vimplus执行完成后再安装~~，接下来需要手动编译ycm。

```bash
cd ~
mv YouCompleteMe.tar.gz ~/.vim/bundle/
cd ~/.vim/bundle/
tar -xvf YouCompleteMe.tar.gz
cd YouCompleteMe
./install.py --clang-completer
```

vimplus将自动安装一些软件，比如说。

 - vim
 - g++ 
 - ctags 
 - cmake
 - python2
 - python3

安装的插件我也部分列出来。

 - [Vundle][5]
 - [YouCompleteMe][6]
 - [NerdTree][7]
 - [nerdcommenter][8]
 - [Airline][9]
 - [auto-pairs][10]
 - [DoxygenToolkit][11]
 - [ctrlp][12]
 - [tagbar][13]
 - [vim-devicons][14]
 - [vim-surround][15]
 - [vim-commentary][16]
 - [vim-repeat][17]
 - [vim-endwise][18]
 - [tabular][19]
 - [vim-dirdiff][20]
 - [vim-coloresque][21]
 - [incsearch.vim][22]
 - [vim-startify][23]
 - [change-colorscheme][24]
 - etc...
 
配置YouCompleteMe
-----

到这一步，安装已经完成，你会发现`~`目录有两个文件，一个是vim的配置文件`.vimrc`，一个是YouCompleteMe的配置文件`[.ycm_extra_conf.py][25]`，一般来说建立一个main.cpp来写C、C++程序来说是没有问题的，都会有语法不全，当你需要写一些项目并涉及到第三方库时，就需要更改`[.ycm_extra_conf.py][26]`了，具体步骤如下。

 1. 将.ycm_extra_conf.py拷贝的项目的根目录。
 2. 更改.ycm_extra_conf.py里面的`flags`变量，添加三方库路径和工程子目录路径。
 
使用vim-devicons
-----

桌面版linux使用[vim-devicons][27]插件会出现乱码，需要设置终端字体为`Droid Sans Mono for Powerline Nerd Font Complete`，使用xshell等工具连接服务器linux的用户就没有必要使用vim-devicons了，可以在插件目录将vim-devicons目录删除，不然会导致`NerdTree`的缩进有问题。

快捷键
-----

vim的插件需要设置好了快捷键才会发挥它的威力，有些插件的快捷键可以查看各自官网，有些快捷键我自己改过的，下面罗列部分插件的快捷键。

 - 显示目录树 `<F3>`
 - 显示函数、变量、宏定义等 `<F4>`
 - 显示静态代码分析结果 `<F5>`
 - .h .cpp 文件快速切换 `<F2>`
 - 转到申明 `<, + u>`
 - 转到定义 `<, + i>`
 - 打开包含文件 `<, + o>`
 - Buffer切换 `<Ctrl + P/Ctrl + N>`
 - 光标位置切换 `<Ctrl + O/Ctrl + I>`
 - 模糊搜索文件 `<Ctrl + f>`
 - Surround `<ys{motion or text-object}{char}/cs{orig_char}{dest_char}/ds{char}>`
 - 注释 `<gcc/gcap/gc/,ca/,cA>`
 - DirDiff `:DirDiff <dir1> <dir2>`
 - 重复 `.`
 - 改变主题 `<F10/F9>`

部分特性截图
-----

### 语法补全

YouCompleteMe就不用多说了，它通过clang编译器提供语法快速补全。
![此处输入图片的描述][28]
 
### 文件搜索

ctrlp提供文件搜索，支持模糊查询。
![此处输入图片的描述][29]

### vim-airline

vim-airline提供漂亮的状态栏支持。
![此处输入图片的描述][30]

### vim-surround

![此处输入图片的描述][31]

### vim-commentary

![此处输入图片的描述][32]

### auto-pairs

![此处输入图片的描述][33]

### incsearch.vim

![此处输入图片的描述][34]

### vim-devicons

![此处输入图片的描述][35]
![此处输入图片的描述][36]
![此处输入图片的描述][37]

### vim-coloresque

![此处输入图片的描述][38]

### vim-dirdiff

![此处输入图片的描述][39]

### vim-startify

![此处输入图片的描述][40]

### Change the colorscheme

![此处输入图片的描述][41]


  [1]: https://raw.githubusercontent.com/chxuan/vimplus/master/screenshots/main.png
  [2]: https://github.com/chxuan/vimplus
  [3]: http://chengxuan.me/2016/10/20/%E8%B6%85%E7%BA%A7%E5%BC%BA%E5%A4%A7%E7%9A%84vim%E9%85%8D%E7%BD%AE-vimplus/
  [4]: http://pan.baidu.com/s/1kVdgsRl
  [5]: https://github.com/VundleVim/Vundle.vim
  [6]: https://github.com/Valloric/YouCompleteMe
  [7]: https://github.com/scrooloose/nerdtree
  [8]: https://github.com/scrooloose/nerdcommenter
  [9]: https://github.com/vim-airline/vim-airline
  [10]: https://github.com/jiangmiao/auto-pairs
  [11]: https://github.com/vim-scripts/DoxygenToolkit.vim
  [12]: https://github.com/ctrlpvim/ctrlp.vim
  [13]: https://github.com/majutsushi/tagbar
  [14]: https://github.com/ryanoasis/vim-devicons
  [15]: https://github.com/tpope/vim-surround
  [16]: https://github.com/tpope/vim-commentary
  [17]: https://github.com/tpope/vim-repeat
  [18]: https://github.com/tpope/vim-endwise
  [19]: https://github.com/godlygeek/tabular
  [20]: https://github.com/will133/vim-dirdiff
  [21]: https://github.com/gko/vim-coloresque
  [22]: https://github.com/haya14busa/incsearch.vim
  [23]: https://github.com/mhinz/vim-startify
  [24]: https://github.com/chxuan/change-colorscheme
  [25]: https://github.com/chxuan/vimplus/blob/master/.ycm_extra_conf.py
  [26]: https://github.com/chxuan/vimplus/blob/master/.ycm_extra_conf.py
  [27]: https://github.com/ryanoasis/vim-devicons
  [28]: https://camo.githubusercontent.com/1f3f922431d5363224b20e99467ff28b04e810e2/687474703a2f2f692e696d6775722e636f6d2f304f50346f6f642e676966
  [29]: https://camo.githubusercontent.com/e15ac916ab9a14dd07135cb2d985cc7333200a38/687474703a2f2f692e696d6775722e636f6d2f614f63774877742e706e67
  [30]: https://camo.githubusercontent.com/ba79534309330accd776a8d2a0712f7c4037d7f9/68747470733a2f2f662e636c6f75642e6769746875622e636f6d2f6173736574732f3330363530322f313037323632332f34346332393261302d313439352d313165332d396365362d6463616461336631633533362e676966
  [31]: https://camo.githubusercontent.com/1f02cead8bdcf894f26b0006c44068a33a7dc8e5/687474703a2f2f6a6f65646963617374726f2e636f6d2f7374617469632f70696374757265732f737572726f756e645f656e2e676966
  [32]: https://camo.githubusercontent.com/2f5cb5bc9a964b0d9e623b5b3aff0314294ac841/687474703a2f2f6a6f65646963617374726f2e636f6d2f7374617469632f70696374757265732f636f6d6d656e746172795f656e2e676966
  [33]: https://camo.githubusercontent.com/372b34413e710cdbc95c5a5c1f901baf9e77791d/687474703a2f2f6a6f65646963617374726f2e636f6d2f7374617469632f70696374757265732f736d617274696e7075745f656e2e676966
  [34]: https://raw.githubusercontent.com/haya14busa/i/master/incsearch.vim/incremental_regex_building.gif
  [35]: https://raw.githubusercontent.com/wiki/ryanoasis/vim-devicons/screenshots/v0.8.x/nerdtree-1.png
  [36]: https://raw.githubusercontent.com/wiki/ryanoasis/vim-devicons/screenshots/v0.8.x/nerdtree-2.png
  [37]: https://raw.githubusercontent.com/wiki/ryanoasis/vim-devicons/screenshots/v0.8.x/nerdtree-3.png
  [38]: https://camo.githubusercontent.com/70916a51f45b5729332803c5de303f6f1849fc50/68747470733a2f2f7261772e6769746875622e636f6d2f676f726f64696e736b69792f76696d2d636f6c6f7265737175652f6d61737465722f73637265656e2e706e67
  [39]: https://raw.githubusercontent.com/will133/vim-dirdiff/master/screenshot.png
  [40]: https://raw.githubusercontent.com/mhinz/vim-startify/master/pictures/startify-menu.png
  [41]: https://raw.githubusercontent.com/chxuan/vimplus/master/screenshots/change-colorscheme.gif

