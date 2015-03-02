Chrome 鲜为人知的秘籍(内部协议)&&Chrome功能指令大全
====

楼主以 Chrome <code>版本 39.0.2171.95 m</code> 为例，耗费2小时的记录：

### [chrome://accessibility][1]

用于查看浏览器当前访问的标签，打开全局访问模式可以查看：各个标签页面的文档系统树

### [chrome://appcache-internals][2]

HTML5应用的离线存储管理，点击 <kbd>View Entries </kbd> 可以查看缓存的站点条目，点击 <kbd>Remove </kbd> 可以删除 html5 站点缓存；如果你没有看到这些选项说明，你还没访问过html5的站点

### [chrome://apps][3]

应用启动器，他可让您直接从桌面启动自己喜爱的应用；如果没安装那就点击获取即可；

### [chrome://blob-internals][4]

二进制大对象储存入口，
BLOB是一个大文件，典型的BLOB是一张图片或一个声音文件，由于它们的尺寸，必须使用特殊的方式来处理（例如：上传、下载或者存放到一个数据库）。如：保存位图、保存XML文档。

### [chrome://bookmarks][5]

书签，这个……通常都会用到，你居然还没管理过书签么……

### [chrome://cache][6]

缓存，不过他只能用于查看，却不能管理……鸡肋啊！
还可以点击某个比如图片文件它会用16进制的方法显示缓存文件，比如：

	HTTP/1.1 200 OK
	Server: nginx-l7/1.4.4
	Date: Sun, 01 Feb 2015 20:25:21 GMT
	Content-Type: image/png
	Content-Length: 2372
	Last-Modified: Fri, 30 Aug 2013 02:29:41 GMT
	Accept-Ranges: bytes
	Via: (null), http/1.1 ctc.jiangsu.ha2ts4.59 (ApacheTrafficServer/4.2.1.1 [cRs f ])
	Age: 35971
	X-Via-CDN: f=Edge,s=ctc.jiangsu.ha2ts4.59,c=60.191.125.155
	00000000: 88 01 00 00 03 00 04 00 04 9b aa 4b ab 6c 2e 00  ...........K.l..
	00000010: 5c de aa 4b ab 6c 2e 00 54 01 00 00 48 54 54 50  \..K.l..T...HTTP
	……

### [chrome://chrome/][7] 和 [chrome://help/][21]

关于 Chrome ，想自动升级的 点他！,搞不懂为什么同一个结果要有俩个一样的指令

### [chrome://about/][0] 和 [chrome://chrome-urls][8]

显示所有 chrome 的相关功能的连接，再吐槽一下：搞不懂为什么同一个结果要有俩个一样的指令

### [chrome://components][9]

组件，看看有啥要升级的，比如隔三差五要你升级的 <code>flash</code>

### [chrome://conflicts][10]

列出了所有已经载入或者将要载入主要程序中的 dll 模块，应该是用于调试的吧

### [chrome://crashes][11]

停用启用崩溃报告，如果启用的话将使用情况统计信息和崩溃报告自动发送给 Google ，为chrome的未来添砖加瓦而启用他把~

### [chrome://credits][12]

第三方软件许可证，除此之外还可以查看某个软件的主页（源码）

### [chrome://devices][13]

查看设备，比如链接的打印机

### [chrome://dns][14]

DNS记录查看，如果有网络故障记得先来找他，chrome 还会定时更新hosts的DNS

### [chrome://downloads][15]

下载文件的记录，下载的文档去哪儿了？

### [chrome://extensions][16]

扩展程序，通常由某些大神开发用于提高工作效率的插件，比如代理工具 <code> Proxy SwitchySharp </code>，无痛上脸书

### [chrome://flags][17]

实验室？对，没专业技能不要修改默认设置，比如 GPU的某些设置

### [chrome://flash/][18]

flash插件的详细信息，顾名思义……

### [chrome://gcm-internals][19]

消息推送服务，啰嗦一下： Google Cloud Messaging for Android 是谷歌新推出的云推送消息服务，简称 <code>GCM</code> 。之前这个服务叫C2DM。
该服务可帮助你将数据从服务端发送至应用。C2DM帮助页上的讯息已经宣布，截至昨日，6月26日C2DM已被正式的GCM批准不再使用。C2DM不再接受新的用户，虽然谷歌将短期维持c2dm的使用。但C2DM开发商将不得不在未来使用的GCM，移除他们现有的应用程序C2DM，用GCM代替。

### [chrome://gpu/][20]

PUG详细信息，楼主只是有空去围观一下而已……

### [chrome://histograms][22]

显示某些性能的柱状图（符号形式），它说：从浏览器启动之前的页面加载，然后再刷新重新加载；有点绕舌头，先来看看它的扩展指令，就明白了：

	chrome://histograms/Accessibility
	chrome://histograms/Apps
	chrome://histograms/AsyncDNS
	chrome://histograms/Aura
	chrome://histograms/Autocomplete
	chrome://histograms/Blacklist
	chrome://histograms/BlinkGC
	chrome://histograms/Cache
	chrome://histograms/Canvas
	chrome://histograms/CertificateType2
	chrome://histograms/ChildProcess
	chrome://histograms/ChildProcessSecurityPolicy
	chrome://histograms/Chrome
	chrome://histograms/CloudPrint
	chrome://histograms/Compositing
	chrome://histograms/Conflicts
	……


### [chrome://history][24]

查看访问的历史记录，如果 Boss 想来看你的这个，我同意你可以威胁他……

### [chrome://indexeddb-internals][25]

查看 html5的本地存储，他会列出使用你本地存储的 html5站点，然后你可以“强制清除”它，或者“下载”他

### [chrome://inspect][26]

* 开发工具调试，

比如 打开 Android 自带 USB 调试并将手机连接电脑，勾选 Discover USB devices 就可以点击左面模拟手机，可以在其中进行键盘/鼠标操作，更简便还有 右击页面审查打开开发工具，点击 手机符号 就可以模拟各类设备

* 其他选项：检查页面 、扩展、App、共享、服务 等

### [chrome://invalidations][27]

失效的调试信息，可以查看调试日志以及内部详细信息

### [chrome://ipc][28]

进程间通信，是 Chrome 内部的信息发送，全称是 Inter-Process Communication ；瞬间 biger 爆棚……

### [chrome://media-internals/][29]

媒体内部数据，用来调试在线播放的一些数据，比如测试 音频流 

### [chrome://memory][30]

查看 chrome 各个 网页标签、插件 消耗的内存，输入这个指令会被转换成 <code>chrome://memory-redirect/</code>，不想吐槽了

### [chrome://memory-internals/][31]

每个页面、插件详细的内存信息；点击 <kbd>Update</kbd> 可以获取json格式的进程，以及可视化的信息：PID、Name(Browser/GPU/TAB/History)、Memory……

### [chrome://nacl][32]

本地客户端 Native Client 环境信息，包括浏览器、操作系统、就、插件等

### [chrome://net-internals][33]

Chrome的http抓包工具，扩展指令如下：

	chrome://net-internals/#export
	chrome://net-internals/#capture
	chrome://net-internals/#import
	chrome://net-internals/#proxy
	chrome://net-internals/#events
	chrome://net-internals/#waterfall
	chrome://net-internals/#timeline
	chrome://net-internals/#dns
	chrome://net-internals/#sockets
	chrome://net-internals/#spdy
	chrome://net-internals/#quic
	chrome://net-internals/#httpCache
	chrome://net-internals/#modules
	chrome://net-internals/#tests
	chrome://net-internals/#hsts
	chrome://net-internals/#bandwidth
	chrome://net-internals/#prerender

以上 每一个都很NB，组成了 神一样的 net-internals 指令
比如说抓取微信 <code>uin</code> （user information）
1、登陆网页版微信
2、打开新页面输入 chrome://net-internals/#events
3、搜索 uin 即可
或者 直接右击网页版微信审查元素 搜索 uin 也可以

### [chrome://newtab][34]

试一下就知道了：打开一个新窗口~

### [chrome://omnibox/][35]

智能地址栏，我觉得还可以翻译成：懂你！他能根据你的输入匹配URL

### [chrome://password-manager-internals][36]

捕获密码管理日志

### [chrome://plugins][37]

可以停用启用相关插件

### [chrome://policy][38]

它用来编辑一些策略，比如 Allow Cross Origin Auth Prompt （允许跨域认证的提示）


### [chrome://predictors][39]

URl输入命中率，结合 <code>omnibox</code> 看看它懂你的程度如何

### [chrome://print][40]

这个使用频率也是相当高的：调用打印功能

### [chrome://profiler][41]

分析器，详细进程的描述都在这里，没事别点 <code>Show all</code>

### [chrome://quota-internals][42]

浏览器对磁盘使用的配额，显示详细可用空间以及各个网站的使用配额

### [chrome://serviceworker-internals][43]

调试服务，说明：打开窗口启动调试 开发人员工具 服务 

### [chrome://settings/][44]

设置，包括，登陆、启动、外观、搜索、其他人、默认浏览器、高级选项

### [chrome://signin-internals][45]

内部标签：包括，基础信息、上一次更新、登陆令牌、账户

### [chrome://stats/][46]

查看浏览器状态

### [chrome://suggestions][47]

有空去提提建议

### [chrome://sync-internals/][48]

各种同步记录

### [chrome://system][49]

系统诊断数据

### [chrome://terms/][50]

查看各种条款

### [chrome://thumbnails/][51]

返回近期浏览的网站的首页快照，以相册的形式

### [chrome://tracing/][52]

追踪访问URL，包括浏览和渲染的过程

### [chrome://translate-internals][53]

内部翻译器，以及你的翻译记录

### [chrome://user-actions/][54]

监听用户行为，比如你的鼠标点击事件，访问url事件都会被记录下来
 

完
----

[0]: chrome://about/ 					"显示所有 chrome 的功能连接"
[1]: chrome://accessibility 			"当前访问的标签"
[2]: chrome://appcache-internals 		"HTML5应用的离线存储管理"
[3]: chrome://apps 						"应用启动器"
[4]: chrome://blob-internals 			"二进制大对象储存入口"
[5]: chrome://bookmarks 				"书签"
[6]: chrome://cache 					"缓存"
[7]: chrome://chrome 					"关于Chrome"
[8]: chrome://chrome-urls 				"显示所有 chrome 的功能连接"
[9]: chrome://components 				"组件"
[10]: chrome://conflicts 				"载入主进程的dll模块"
[11]: chrome://crashes 					"停用启用崩溃报告"
[12]: chrome://credits 					"第三方软件许可证"
[13]: chrome://devices 					"查看设备"
[14]: chrome://dns 						"DNS记录"
[15]: chrome://downloads 				"下载记录"
[16]: chrome://extensions 				"插件"
[17]: chrome://flags 					"实验室"
[18]: chrome://flash 				    "关于flash"
[19]: chrome://gcm-internals 			"消息推送服务"
[20]: chrome://gpu 						"GPU信息"
[21]: chrome://help 					"关于Chrome"
[22]: chrome://histograms 				"显示某些性能的柱状图"
[24]: chrome://history 					"查看历史记录"
[25]: chrome://indexeddb-internals 		"html5本地储存"
[26]: chrome://inspect 					"开发工具调试"
[27]: chrome://invalidations 			"失效的调试信息"
[28]: chrome://ipc 						"进程间通信"
[29]: chrome://media-internals 			"媒体内部数据"
[30]: chrome://memory 					"内存查看"
[31]: chrome://memory-internals 		"详细内存查看"
[32]: chrome://nacl 					"本地客户端环境信息"
[33]: chrome://net-internals 			"http抓包工具"
[34]: chrome://newtab 					"打开一个新窗口"
[35]: chrome://omnibox 					"智能地址栏"
[36]: chrome://password-manager-internals "密码管理日志"
[37]: chrome://plugins 					"插件"
[38]: chrome://policy 					"编辑策略"
[39]: chrome://predictors 				"URl输入命中率"
[40]: chrome://print 					"打印"
[41]: chrome://profiler 				"分析器"
[42]: chrome://quota-internals 			"使用配额"
[43]: chrome://serviceworker-internals 	"内部调试服务"
[44]: chrome://settings 				"设置"
[45]: chrome://signin-internals 		"内部标签"
[46]: chrome://stats 					"状态"
[47]: chrome://suggestions 				"建议"
[48]: chrome://sync-internals 			"同步记录"
[49]: chrome://system 					"关于系统"
[50]: chrome://terms 					"查看条款"
[51]: chrome://thumbnails 				"网站快照相册"
[52]: chrome://tracing 					"追踪"
[53]: chrome://translate-internals 		"翻译器"
[54]: chrome://user-actions 			"监听用户行为"

