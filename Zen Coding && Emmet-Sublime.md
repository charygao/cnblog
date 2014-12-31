Zen Coding && Emmet-Sublime
====================

Sublime Text 插件之：Emmet，旧版称：ex-Zen Coding 更名之后增加了CSS3和HTML5许多新特性。项目地址也从 code.google 移 github. <img align="absmiddle" alt="octocat" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/octocat.png" title="octocat" width="20px">

#### 安装：

> + Package 方式就不再赘述了（如何安装请移步：[安装package][0]）
> + 安装了 控制包后搜索 __Emmet__ 即可

##### 手动安装

1. __下载 Emmet Sublime__ ，的github项目地址 [emmet][2]；你可以 [clone][5] 或者直接 [下载][4]<img align="absmiddle" alt="gift" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/gift.png" title="gift" width="20px"> 
2. __文件夹手动安装__ ，下载完成后打开 <code>Preferences > Browse Packages </code> 直接放入文件夹 <code>emmet-sublime-master</code>，重启 ST 菜单栏 <code>Preferences > Package Settings > emmet </code>已经出现，没有？那就没办法了……
3. __下载安装 PyV8 Binaries__ ，注意你的操作系统，由于 *git下载源不稳定* 楼主特地整理了各个操作系统的 V8 : 移步 **[百度网盘][8]** 吧 <img align="absmiddle" alt="gift" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/gift.png" title="gift" width="20px"> 

安装方式 同第2步 放入文件夹 <code>PyV8</code> 选择自己操作系统相对应的 V8 再放入 <code>PyV8</code> 文件夹中 比如 <code>pyv8-win64</code> ，然后 重启下 ST 

>  github V8项目最新版下载地址： [PyV8 download][7]

4.__尝试一下__ 敲<code> html:5 </code> 按 <kbd>Ctrl+e</kbd> 看看发生了什么（当然<kbd>Tab</kbd>也可以）
在敲一个 <code>div.test.wrap>ul#nav>li.item$*4>a{Item $}</code> 开心吧！难过？那就没办法了~

> + 注：在未装 Emmet 前 ST 支持

比如： <code>div.wrap</code> 按 <kbd>Tab</kbd> 后输出

    <div class="wrap"></div>

但是： <code>div.wrap.test</code> 按 <kbd>Tab</kbd> 后输出

    div<wrap>test</wrap>

安装成功 Emmet 后输出 

    <div class="wrap test"></div>

那么 <code>div.test.wrap>ul#nav>li.item$*4>a{Item $}</code> 还需要解释么，和css的写法基本类似吧， __\*4__ 自然是重复 __4__ 次， __{Item $}__ 表示 __a__ 标签中的 内容

具体可以参考 [Emmet 官网][6]

> E
> 元素名（div、p）；
> E#id
> 带Id的元素（div#content、p#intro、span#error）；
> E.class
> 带class的的元素（div.header、p.error）,id和class可以连着写，div#content.column
> E>N
> 子元素（div>p、div#footer>p>span）
> E*N
> 多项元素（ul#nav>li*5>a）
> E+N
> 多项元素
> E$*N
> 带序号的元素

![结果欣赏](http://images.cnitblog.com/blog/531703/201412/301627062638501.gif)





[0]: http://www.cnblogs.com/highsea90/p/4193421.html                "package install"
[1]: https://github.com/emmetio/pyv8-binaries                       "pyv8 binaries"
[2]: https://github.com/sergeche/emmet-sublime/                     "emmet sublime"
[4]: https://github.com/sergeche/emmet-sublime/archive/master.zip   "download master"
[5]: https://github.com/sergeche/emmet-sublime.git                  "clone emmet"
[6]: http://docs.emmet.io/                                          "emmet docs"
[7]: https://github.com/emmetio/pyv8-binaries/blob/master/README.md "pyv8 download"
[8]: http://pan.baidu.com/s/1qW7G2C0                                "百度网盘 V8集合"