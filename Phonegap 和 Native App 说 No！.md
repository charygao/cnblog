Phonegap 和 Native App 说 No！
========

目前要开发 Web App 还是比较轻松的，接下来以 **Web前端开发工程师** 的角度来一个 <code> First Blood </code>

### 一、开发环境：

这里先以 Windows 为例：

* ![Node js 安装10.35][15] **Node.js** [官网下载][1]，[百度云盘 版本号：10.35 for x64][4]

安装完成后（目前已经整合了 *NPM* ），最好在系统变量 <kbd>Path</kbd> 中加入安装路径；

    $ node -v
    v0.10.35
    $ npm -v
    1.4.28

* ![ADT][14] **Adroid [ADT Bundle for Windows][2]** ，<img align="absmiddle" alt="gift" height="20px" src="https://assets-cdn.github.com/images/icons/emoji/gift.png" title="gift" width="20px">[百度云下载 sdk版本号:135.1641136][3]：

下载完成后设置路径，比如： <code>F:\Android SDK\adt-win-x64-135\</code> 用来安装 SDK 软件
<code> F:\Android SDK\local-sdk</code> 用来安装 sdk 组件 （都随意……）

安装不是很慢是特别慢，幸好是无人值守型……

在安装完成后，第一次启动的会显示 <code>Fetching Android SDK component information</code>，然后在 <code> Setup Wizard - Downloading Components </code> 界面下面开始下载 Andorid SDK，如果感觉被“qiang”，（组件下载特别的缓慢甚至中断报错）[请移步这里][9]

或者请参考网友们的方法：
a：关闭安装向导（不行？那就结束进程吧）；
b：打开 Android Studio 安装目录的 <code>F:\Android SDK\adt-win-x64-135\bin</code> 目录下面的 <kbd>idea.properties</kbd> 文件，添加一条配置项：禁用开始运行向导；

    # by highsea 2015-1-4
    disable.android.first.run=true



* ![jdk安装 7u71][13] **JDK**  [官网下载][6]，[百度云盘 jdk 7u71 x64][6]

如果你还在用6请升级到7，不然在安装 Android SDK 会报错

接着 <kbd>Path</kbd> 中加入如下路径：

    %java_home%/bin;

设置系统变量，变量名： <code> JAVA_HOME </code> ；变量值： 

    C:\Program Files\Java\jdk1.7.0_71   

* ![Sublime text 安装][16] **IDE/编辑器** ，自选，推荐 [Sublime Text 2 :ST2 2.0.2 X64 ][7] 相关[插件请移步这里][10]

* ![Eclipse for jee 安装][17] **安装Eclipse** [官网下载][12]，[百度云盘 jee][19]

> 区别： Eclipse有许多版本，最常见的就是 Eclipse IDE for Java Developers 与Eclipse IDE for Java EE Developers 他们都可以用来开发 android 应用，推荐安装后者（后者包含前者），后者主要的有点还有多了一些 web 插件，更适合开发网络应用。

* ![phonegap][18] **安装 PhoneGap** 


    $ cd /f/Android\ SDK/&&npm install -g phonegap



### 三、新建项目

1、配置 Android Studio 

    configure > project Defaults > Project Structure 

设置Android SDK和JDK的路径：
> 第一个是 sdk 组件目录 <code> F:\Android SDK\local-sdk</code>
> 第二个是 jdk 目录 <code>C:\Program Files\Java\jdk1.7.0_71</code> 






[1]: http://nodejs.org/download/                    "Nodejs 下载"
[2]: http://developer.android.com/sdk/index.html    "Android SDK 下载"
[3]: http://pan.baidu.com/s/1gdAGMT9                "135.1641136 百度云"
[4]: http://pan.baidu.com/s/1dDcUUvn                "10.35 for x64 nodejs"
[5]: http://pan.baidu.com/s/1jGHwqGI                "JDK 7u71 for x64"
[6]: http://www.oracle.com/technetwork/java/javase/downloads/index.html "jdk 官网"
[7]: http://pan.baidu.com/s/1eQ5tad8                "ST2 2.0.2 X64 "
[8]: http://phonegap.com/install/                   "PhoneGap 安装页面"
[9]: https://github.com/highsea/Hosts               "Hosts 修改"
[10]: http://www.cnblogs.com/highsea90/p/4201392.html "Sublime Text 插件"
[11]: http://developer.android.com/tools/sdk/eclipse-adt.html#downloading    "eclipse-adt 插件"
[12]: http://www.eclipse.org/downloads/             "Eclipse 下载安装"

[13]: http://images.cnitblog.com/blog/531703/201501/041745526841554.jpg "Java jdk安装"
[14]: http://images.cnitblog.com/blog/531703/201501/041732351225347.png "Android ADT 安装"
[15]: http://images.cnitblog.com/blog/531703/201501/041737429506604.png "Nodejs 10.35 安装"
[16]: http://images.cnitblog.com/blog/531703/201501/041743362782027.jpg "Sublime Text 神器"
[17]: http://images.cnitblog.com/blog/531703/201501/041733111842663.png "Eclipse For jee-win-X64"
[18]: http://images.cnitblog.com/blog/531703/201501/041732081695982.png "PhoneGap 安装"

[19]: http://pan.baidu.com/s/1o69gBvc               "Eclipse for jee 百度云"