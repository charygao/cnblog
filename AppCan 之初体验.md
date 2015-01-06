AppCan 之初体验
========

### 平台概述

什么是AppCan [移步这里][1]，楼主的一句话：可以匹敌 [Phonegap][2] 、[Titanium][3] 、[Sencha Touch][4] 、[MUI][5] 的跨平台移动开发框架，可用于开发 Web App 的 **国产、免费、不开源** 框架。
但除 Appcan ，其他框架都是免费的而且开源（开源确实很重要……不过国产软件不开源似乎也能理解），[MUI][5]也是国产的，个人也是特别喜欢，因为这样至少可以少看一些鸟语的 API 文档了。这里有一篇 [AppCan VS PhoneGap - 对比两大移动开发平台][6]，虽各有利弊但 AppCan 似乎更胜一筹；

> AppCan 生成的 APP 体积大似乎是最大的弊端了

有时间 楼主会整理一篇 AppCan VS MUI 的对比；不过 就凭 *打包无需native开发环境* 这一点，我想应该有很多 Web 工程师喜欢不得了，ADT 的配置 以及 SDK Manager 的组件下载（被qiang）是非常头疼…… 

### 开发工具/IDE

概述就省略了吧……

##### 下载安装注册

[官网下载_V3.1.4][7]，[百度云下载_V3.1.4][8]

> 注：1、目前 **不支持** xp sp2 及其以下操作系统和Mac OS X操作系统。
> 	2、使用 IDE 需要注册账号（同 HBuilder ）。

##### 创建APP - **first Blood**

> 登陆 [应用管理][9] 先创建一个 应用比如： <code>firstblood</code> 进入管理页面可以获得它的 应用ID、应用Key、svn

依次进行  文件 > 新建 > AppCan项目 > 新建项目 > 下一步 > 填写 “项目名称”、“应用名称、“应用ID”、“应用key”  > 下一步 > 选择空模板 > 下一步 > 设置模板主题 > 完成 。

##### 调试、运行App

设置修改完成 config.xml 文件后，点击菜单的 启动调试服务中心（即，本机localhost或者局域网192.168.1.2调试，http端口3000） ![启动WEB调试][10] 或者生成App调试 ![生成App调试][11] 或者真机调试 ![真机调试][12] 

![启动界面][14]

AppCan 可以设置SVN，右击项目，选择 team > 共享 即可

> 关于调试，推荐安装模拟器 BlueStacks App Player 一个就行抛开 ADT 吧！


#### 开发 First Blood

安装好 生成的 App ，启动虚拟机，输入 本机IP开始，比如：192.168.1.2 （端口号不用）……

接下来修改 index.html 和 index_content.html ，如果喜欢 Sublime Text 的同学依旧可以在此返回 ST 修改，保存即可；

实时查看效果：

![模拟器效果][15]

一血结束，就是这么简单，
要不来回想下 Phonegap 和 MUI 吧~

--------
参考 AppCan 文档中心；[点此查看详情][16]


[1]: http://newdocx.appcan.cn/index.html?templateId=392 	"什么是 AppCan"
[2]: phonegap.com/ 											"Phonegap"
[3]: http://www.appcelerator.com/titanium/ 					"Titanium"
[4]: http://www.sencha.com/ 								"Sencha Touch"
[5]: http://dcloudio.github.io/mui/ 						"MUI"
[6]: http://my.oschina.net/liux/blog/65119 					"AppCan VS PhoneGap 对比"
[7]: http://download.appcan.cn/appcan_sdk/AppCan_IDE_Personal_Setup_V3.1.4_201412261245.exe 														"官网下载_V3.1.4 "
[8]: http://pan.baidu.com/s/1i3L2h05 						"AppCan IDE V3.1.4"
[9]: http://dashboard.appcan.cn/app/ 						"AppCan 应用管理"
[10]: http://images.cnitblog.com/blog/531703/201501/061502580004608.jpg "启动WEB调试"
[11]: http://images.cnitblog.com/blog/531703/201501/061505131252094.jpg "生成App调试"
[12]: http://images.cnitblog.com/blog/531703/201501/061506163908932.jpg "真机调试"
[13]: http://xz5.cr173.com/soft1/BlueStacksAppPlayer.zip 				"BlueStacks App Player"
[14]: http://images.cnitblog.com/blog/531703/201501/061535102814887.jpg "启动界面"
[15]: http://images.cnitblog.com/blog/531703/201501/061546299845367.jpg "模拟器效果"
[16]: http://newdocx.appcan.cn/index.html 					"AppCan 文档中心"


