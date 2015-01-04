Sublime Text 插件之：MarkDown <img align="absmiddle" alt="octocat" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/octocat.png" title="octocat" width="20px">
喜欢写文档的同学应该离不开 MarkDown ，ST（Sublime Text）的插件 Markdown Preview 就支持实时在浏览器中预览<code>preview in browser</code>，或者转换成html文档保存的功能<code>Export HTML in Sublime Text</code>。

#### 安装 Package

安装 包控制器（Package Control） 用于安装 ST 的各类插件；如同 **nodejs** 的 **npm** 一样；已经安装的同学请忽略之；打开ST后按 <kbd>ctrl+`</kbd> 找不到？那么点击查单 <code>View > Show Console </code>后出现控制台，请选择以下代码回车执行。注意查看提示哦，
安装成功后最好重启（针对于2） ST ；查看菜单栏 <code>Preferences > Browse Packages > Markdown Preview </code> 已经出现，没看到？那就重复以上操作吧！

> 猛击这里查看 [packagecontrol 官网](https://packagecontrol.io/ 'packagecontrol') 也可以搜索各类插件 


###### For Sublime Text 3

    import urllib.request,os,hashlib; h = '2deb499853c4371624f5a07e27c334aa' + 'bf8c4e67d14fb0525ba4f89698a6d7e1'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

###### For Sublime Text 2

//推荐<img align="absmiddle" alt="heart" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/heart.png" title="heart" width="20px">

    import urllib2,os,hashlib; h = '2deb499853c4371624f5a07e27c334aa' + 'bf8c4e67d14fb0525ba4f89698a6d7e1'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')

//或者

    import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')

安装成功后 Markdown Preview 默认没有浏览器预览快捷键，可以自行设置；<code>Preferences -> Key Bindings User </code> 打开后在 中括号中添加以下代码：

#### markdown 的快捷键

快捷键可以随意组合 这里默认 <kbd>Alt+M+D</kbd>

    { "keys": ["alt+m+d"], "command": "markdown_preview", "args": { "target": "browser"} }


#### MD语法高亮和mathjax支持

在<code>Preferences ->Package Settings->Markdown Preview->Setting Default </code>Default中的第47行和57行，开启即可

    { 
        "enable_mathjax": true ,
        "enable_highlight": true,
    }

> 注：ST 3 的一个大坑：__Setting Default 无法修改，User 即便修改重启也不生效！__

#### 写作与预览

markdown 常用语法参考请移步 [MarkDown官网](http://daringfireball.net/projects/markdown/ )

预览？就是你设置的快捷键啦~

转存？以Chrome浏览器为例：按 <kbd>Ctrl+P</kbd> (找不到？那就右击页面空白处选择打印即可)，选择目标打印机 还可以另存为PDF

#### 删除
以下为 Windows：

_插件删除_：找到：<code>X:\Users\YourName\AppData\Roaming\Sublime Text 2(3)\Packages\</code>将Markdown 和 Markdown Preview 俩个文件夹<kbd>Shift+Delete</kbd>强制删除

_ST删除_: 找到 <code>X:\Program Files\Sublime Text 2(3)</code>清空该目录

> 注：楼主删除重装后出现一大坑：无论如何设置语法不能高亮，且实时刷新无效果

#### 附件：ST2 & ST3 百度网盘 <img align="absmiddle" alt="gift" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/gift.png" title="gift" width="20px">：

ST2 __2.0.2__  X64 版本 [猛击](http://pan.baidu.com/s/1eQ5tad8)

ST3 __3065__  X64 版本 [猛击](http://pan.baidu.com/s/1pmhmY)

   [1]: https://assets-cdn.github.com/images/icons/emoji/heart.png  "heart"