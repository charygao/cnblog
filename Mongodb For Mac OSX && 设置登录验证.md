Mongodb For Mac OSX && 登录验证
====

题外话：尽管有不少人贴出了 《我不用mongodb的十大理由》 等系列文章，但是 NoSQL 的发展不会因此而止步， mongodb 是 NoSQL 的典型代表，楼主还是抱乐观态度的，有人讨厌是好事，尽管mongodb依然在不断的更新中……

### 1.环境：

MacBook Pro ： OS X 10.9.5 (13F34)
Mongodb ： 2.6.0 

### 2.安装和启动：

A： 包管理工具 **自动化安装** [小心被墙，下载中建议去煮几杯咖啡]

	$ brew install mongodb
	//记得可以先更新 homebrew
	//$ brew update

安装完成后，启动MongoDb

	mongod —config /usr/local/etc/mongod.conf

B： 文件方式 **解压安装** [百度网盘分分钟搞定]

点击下载： [Mongodb OSX 2.6.0 zip 压缩包][1] 

解压到随意位置， 比如 <code>/usr/local/var/www/mongodb-osx-x86_64-3.0.0/</code> 进入 mongodb－osx…… 目录创建两个文件夹 **data/db** (数据)和 **data/log** (日志)

然后轻松启动，比如：

	$ cd /usr/local/var/www/mongodb-osx-x86_64-3.0.0/bin/
	$ mongod --dbpath "/usr/local/var/www/mongodb-osx-x86_64-3.0.0/data/db"
	……
	……
	2015-05-18T13:49:15.660+0800 [initandlisten] journal dir=/usr/local/var/www/mongodb-osx-x86_64-3.0.0/data/db/journal
	2015-05-18T13:49:15.660+0800 [initandlisten] recover : no journal files present, no recovery needed
	2015-05-18T13:49:15.843+0800 [initandlisten] waiting for connections on port 27017

启动成功，端口号是 27017 成功， **大多数人到这里应该就没有下文了～** ，(比如，端口怎么改？ 接着看……)

### 3.启动参数

例子：

	mongod --dbpath="安装路径/data/mongodb" --logpath="安装路径/data/mongodb/logs/mongodb.log" --logappend --auth --port=27017 --fork

没事，你先试一下，我等你 ^_^

解释：
	
	mongod  	: 启动程序命令
    --dbpath 	: 的数据库存放路径
    --logpath 	: 的日志文件路径
    --logappend : 以追加方式，写日志文件
    --auth      : 是否进行用户认证，加上后,MongoDB会使用用户认证方式登录。
    --port      : 端口号，可以自定义，默认 27017
    --fork      : 服务是否以后台运行的方式运行


### 4.设置登录权限

进入到 bin 目录执行 mongo 就可以，比如楼主的：

	$ cd /usr/local/var/www/mongodb-osx-x86_64-3.0.0/bin/&&mongo

接下来可以熟悉下操作一些常用指令，比如增删改查“CURD”，这里就不一一列举了，传送门：  [mongodb for windows][3]
这里说一下如何 **添加登录权限** ，先贴一段 code：

	> show dbs
	admin    (empty)
	hi-blog  0.078GB
	local    0.078GB
	session  0.078GB
	> use hi-blog
	switched to db hi-blog
	> show collections
	apikeys
	classifys
	system.indexes
	users
	> db.addUser('root','root123')
	WARNING: The 'addUser' shell helper is DEPRECATED. Please use 'createUser' instead
	Successfully added user: { "user" : "root", "roles" : [ "dbOwner" ] }
	> show collections
	2015-05-18T14:24:36.802+0800 error: {
		"$err" : "not authorized for query on hi-blog.system.namespaces",
		"code" : 13
	} at src/mongo/shell/query.js:131
	> db.auth('root','root123')
	1
	> show collections
	apikeys
	classifys
	system.indexes
	users
	> show users
	{
		"_id" : "hi-blog.root",
		"user" : "root",
		"db" : "hi-blog",
		"roles" : [
			{
				"role" : "dbOwner",
				"db" : "hi-blog"
			}
		]
	}
	> _

以上先展示了数据库的“表”（databases）然后切换到了某个“集”（collections）然后再看下这个集合下的数据“行”（document），在没有添加（addUser）管理员前 collections 随便看，一旦添加了 管理员 则需要认证后（auth）才能查看
**［注意：大前提是在启动mongodb服务时添加了 --auth 用户认证参数］**


### 5.mongoose 登录验证

类似的 轻量级的nodejs mongodb驱动有很多，比如：

[mongoose][4], [node-mongodb-native][6], [mongoskin][7], [node-mongolian][5], [mongous][8], [mongojs][9]

他们的关系据说是这样：


											---- mongoose (ORM框架)
				---- node-mongodb-native --|
	mongodb  --|							---- mongoskin (mongo shell)
				---- mongous



这里以mongoose为例，举例登录验证

	mongodb://你的账号:密码@host ip:端口号/设置登录权限的数据库
	mongodb://admin:123456@192.168.1.100:27018/yourdb

这还是很轻松的，什么？上下文也要？如下：

	//mongodb操作.js
	var mongoose = require('mongoose'),
	    config = require('./../db/config');
	db = mongoose.createConnection();

	//设置用户名密码端口数据库
	db.openSet(config.dbLogin);
	// 链接错误
	db.on('error', function(error) {
	    console.log(error);
	});
	……

	//config.js
	//需要登录的mongodb
	var dbLogin 		= 'mongodb://admin:123456@192.168.1.100:27018/yourdb';
	……
	exports.dbLogin 	= dbLogin;
	……

----
完



[1]: http://pan.baidu.com/s/1i37tBGL 									"Mongodb OSX 2.6.0 zip"
[2]: http://docs.mongodb.org/ecosystem/tools/administration-interfaces/ "可视化工具一览"
[3]: http://www.cnblogs.com/highsea90/p/4195927.html 					"mongodb for windows"
[4]: https://github.com/Automattic/mongoose 							"mongoose"
[5]: https://github.com/marcello3d/node-mongolian 						"node-mongolian"
[6]: https://github.com/mongodb/node-mongodb-native 					"node-mongodb-native"
[7]: https://github.com/kissjs/node-mongoskin 							"mongoskin"
[8]: https://github.com/amark/mongous 									"mongous"
[9]: https://github.com/mafintosh/mongojs 								"mongojs"