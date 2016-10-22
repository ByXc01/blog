---
title: 使用hexo+github搭建个人博客
date: 2016-10-19 17:24:04
categories: 制作个人博客
tags: 
    - hexo
    - github
---

![此处输入图片的描述][1]

前言
-----

上大学期间都没有做笔记、写博客的习惯，工作以后学的东西多了，接触的东西多了，才发现做笔记是一件很重要的事情，做笔记可以将自己的想法、思路写下来，方便以后查阅，俗话说好记性不如键盘党，做笔记、写博客也可以让自己学会总结、学会分享，今年年初才开始使用cnblogs来写博客，账号已经申请了两年多了，大学期间一直没用，cnblogs写了一段时间发现网上一些大牛都有自己的博客，因为我比较喜欢折腾和装X，所以我也打算搭建一个博客，我不是做web方向的，也不懂jsp、asp.net、php（世界上最好的语言）、webpy等语言和技术（大学时学过，后来就忘了），之前我看到我同学基于[WordPress][2]搭建了一个[博客][3]，这个需要数据库啊，服务器之类的，感觉有点麻烦，后来在网上查阅资料看到[hexo][4]、[jekyll][5]配合github就可以用来搭建博客，github作为服务器这样还省去了租用服务器的费用，有人会问国内的[coding][6]也可以作为部署服务器啊，还快些，我只想说信仰不同，不相为谋，存储图片我也是用的github，没有用七牛的，最后我选择的hexo + github方案来制作个人博客，我是在ubuntu上搭建的，在windows和mac上搭建的朋友本篇博客还是有参考意义，下面是详细的制作过程(个人博客也发表了[《使用hexo+github搭建个人博客》][7])。

<!--more-->

安装git
-----

部署服务器需要使用github，所以git成了必要工具。

```bash
sudo apt-get install git
```

安装node.js
-----

我直接在[node.js][8]的[官网][9]下载二进制包来安装的，下载过后，解压，设置软链接。

```bash
ln -s /your/nodejs/dir/bin/node /usr/local/bin/node
ln -s /your/nodejs/dir/bin/npm /usr/local/bin/npm
```

将上面路径替换成你的nodejs真实路径，也可以直接将node可执行文件拷贝到`/usr/local/bin`目录下。

安装hexo
-----

```bash
sudo npm install -g hexo-cli
```

安装hexo需要使用npm包管理器来安装，安装好后运行hexo命令，控制台提示说找不到该命令，让我郁闷了一哈，后来才发现hexo命令在`/your/nodejs/dir/bin/`目录下，还是老办法，设置软链接。

```bash
ln -s /your/nodejs/dir/bin/hexo /usr/local/bin/hexo
```

建立站点
-----

```bash
hexo init blog
```

blog目录就是你的站点根目录，目录里面的`_config.yml`是`站点配置文件`，后面还会说到`主题配置文件`，每一个主题都用一个`_config.yml`文件，不要搞混了，到目前为止博客环境已经搭建完成。

本地调试
-----

博客搭建好了，没有run起来感觉心里是虚的，接下来我们把博客run起来看，首先生成静态页面。

```bash
hexo generate(可以缩写成g)
```

启动本地服务，在浏览器输入http://localhost:4000就可以看效果了。

```bash
hexo server(可以缩写成s)
```

![此处输入图片的描述][10]
看到上图出现，说明搭建博客成功。

配置github
-----

hexo生成的静态页面是要上传到github上面的，所以需要配置好github，首先需要在github上建立一个仓库，仓库名格式是`username.github.io`，比如我的就是`chxuan.github.io`，不要乱取，不然配置不成功。之后编辑`站点配置文件`在末尾加入。

```bash
deploy:
  type: git
  repo: https://github.com/chxuan/chxuan.github.io.git
  branch: master
```

repo行需要替换成你自己的仓库路径，保存之后运行如下命令。

```bash
npm install hexo-deployer-git --save
hexo deploy(可以缩写成d)
```

至此hexo已经关联好了github，在浏览器输入http://username.github.io/，比如我的是http://chxuan.github.io/就可以浏览了，github默认提供的是一个二级域名，你也可去阿里云购买域名，替换掉github提供的。

发表文章
-----

```bash
hexo new "xxxxxxxx" 
hexo clean
hexo generate(可以缩写成g)
hexo deploy(可以缩写成d)
```

以上是发表文章的步骤，执行hexo new 之后会在站点目录的`source/_posts/`目录下生成`.md`结尾的博客，我用的[Cmd Mardown][11]来写博客的。

创建一个标签页
-----

```bash
hexo new page "about" 
hexo clean
hexo generate(可以缩写成g)
hexo deploy(可以缩写成d)
```

上面创建了一个关于我的标签页并部署到github服务器上。


设置主题
-----

我使用的是[NexT][12]主题，目前github星星数最多的一个主题，主题界面一般，主要是文档齐全吧，所以很受人们欢迎，我也建议新手使用该主题，主题配置参考[NexT官方文档][13]。
![此处输入图片的描述][14]

集成第三方插件
-----

若想要别人评论你的博客、查看访问次数、搜索博客等功能需要第三方插件支持，你可以参考[NexT官方文档][15]。

多电脑发布博客
-----

公司电脑和家用电脑都可以写博客，当环境搭建好后，怎么进行文章同步呢，我使用的是github，我在github上创建了一个名为blog的仓库用来存放博客文件，你需要将本地站点blog目录进行`hexo clean`之后，`hexo clean`执行过后就是删除`public`里面生成的静态页面等操作，将剩下的文件放入github同步就可以了。

参考文章
-----

 1. [hexo官方文档][16]
 2. [NexT主题官方文档][17]


  [1]: https://raw.githubusercontent.com/chxuan/images/master/blog/2016/10/hexo.png
  [2]: https://cn.wordpress.org/
  [3]: http://www.lampnick.com/
  [4]: https://hexo.io/zh-cn/
  [5]: http://jekyll.com.cn/
  [6]: https://coding.net/
  [7]: http://chengxuan.me/2016/10/19/%E4%BD%BF%E7%94%A8hexo-github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/
  [8]: http://nodejs.cn/
  [9]: http://nodejs.cn/
  [10]: https://raw.githubusercontent.com/chxuan/images/master/blog/2016/10/hexo2.png
  [11]: https://www.zybuluo.com/mdedito
  [12]: http://theme-next.iissnan.com/
  [13]: http://theme-next.iissnan.com/theme-settings.html
  [14]: https://raw.githubusercontent.com/chxuan/images/master/blog/2016/10/hexo3.png
  [15]: http://theme-next.iissnan.com/third-party-services.html
  [16]: https://hexo.io/zh-cn/docs/
  [17]: http://theme-next.iissnan.com/getting-started.html
